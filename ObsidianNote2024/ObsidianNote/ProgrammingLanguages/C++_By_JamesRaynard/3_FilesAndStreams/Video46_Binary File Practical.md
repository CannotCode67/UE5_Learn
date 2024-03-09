
This video shows how to create the data in bitmap format and transmit that data in binary form to a bmp file, then open the file with any picture-related software to see that our data is truly valid to be displayed as bitmap image. To get the details, we should watch the video again.

One concept mentioned but not quite explained in the video 45 is the binary file structure. The data in a binary file needs to be structured in a meaningful way, so applications know how to use the data. When we write data in binary form to a file, we pick a file format. File formats like `.jpg`, `.bmp`, `.zip`, etc. are designed by others. They are like protocols which are standards we all agree. If you put your binary data in one format, then other applications can extra and use your data correctly given they know this format. So, you can create your own format without any problem, the hard part is to ask applications written by others to accept your format. Formats mentioned above are very well known, and widely accepted, that's why you will find many applications can open files in those formats.

Now, consider serialization of a video game. When the game is exited, we write the game states into a binary file on disk so that when we launch the game next time, the player's progress will be recreated based on the binary file our game read. The data can be in a format designed by our own, because the only application that will use this data is our game.

The best way to work with a certain file format is to create a struct whose data members corresponding to the fields in the file format. Take bitmap format as an example.
The bitmap format starts with a file header.
```
#pragma pack(push, 2) // bitmap format assume we are on a 16-bit system
struct bitmap_file_header {
	char header[2] {'B', 'M'};
	int32_t file_size;
	int32_t reserved{0};
	int32_t data_offset;
}
#pragma pack(pop)
```
Then followed by a info header.
```
struct bitmap_info_header {
	int32_t header_size{40};
	int32_t width;
	int32_t height;
	... // various default values
	int32_t important_colours{0};
}
```
We also need to define pixel.
```
struct pixel { // bitmap format is designed by Microsoft, so BGR, not RGB
	uint8_t blue;
	uint8_t green;
	uint8_t red;
}
```
When we use, we write a class representing bitmap image.
```
class bitmap {
	private:
		int width{800};
		int height{600};
		std::string filename;
		std::vector<pixel> pixels;
	public:
		bitmap(std::string filename): filename(filename), pixels(width*height) {}
		void set_pixel(int x, int y, pixel p);
		void set_row(int rownum, pixel p);
		void set_all(pixel p);
		bool write();
}
```
The only implementation we need to see is `write()`.
```
bool bitmap::write() {
	bitmap_file_header file_header;
	bitmap_info_header info_header;
	file_header.file_size = sizeof(bitmap_file_header) + 
							sizeof(bitmap_info_header) +
							width * height * sizeof(pixel);
	file_header.data_offset = sizeof(bitmap_file_header) + 
							  sizeof(bitmap_info_header);
	info_header.width = width;
	info_header.height = height;
	ofstream ofile(filename, fstream::out | fstream::binary);
	if (ofile.is_open()) {
		ofile.write(reinterpret_cast<char*>(&file_header), sizeof(bitmao_file_header));
		ofile.write(reinterpret_cast<char*>(&info_header), sizeof(bitmao_info_header));
		ofile.write(reinterpret_cast<char*>(&pixels.data()), pixels.size() *sizeof(pixel));
	}

	if (!ofile) return false;
	ofile.close();
	return true;
}
```
We see in `write()`, the binary data is transmitted in three parts sequentially, the file header, info header, then the pixel data. Our bitmap class contains member functions and some fields that are useful for manipulating the bitmap data, but they are not part of the bitmap format. That's why struct is useful, as normally they only contain fields not functions. And they can be used as member field in class to keep the class field concise. When output, we just dump the necessary struct data, which is so much easier than trying to pick the format data out of a bunch of mixed data of a whole class. This also reveals the aspect of file and file format. They are better when only contain data, represented by fields. Leave the functions or ways to manipulate data in applications.




