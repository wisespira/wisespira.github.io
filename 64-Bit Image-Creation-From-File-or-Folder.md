This section provides instructions for generating Base64-encoded images from either a single image file or a folder of images. Both scripts are written in Python and include optimizations to reduce the encoded string size below the 32,767-character limit, ensuring compatibility with Power BI.

​​Creating a Base64 encoded image from an Image File

The following Python script utilizes the base64 library and the PIL package to handle image processing. It begins by importing necessary modules and specifying paths for both the input PNG image and the output CSV file. The script sets a character limit for the base64 encoded string and defines a function to compress the image. The original image is opened, and a loop is initiated to iteratively compress and encode the image until the base64 string meets the character limit. The script then writes the resulting base64 string to a specified CSV file, including an optional header row.

Make sure to update the image_path value before running:

import base64
        from PIL import Image
        from io import BytesIO
        import csv  # Ensure the csv module is imported
        
        # Path to your PNG image
        image_path = "Example.png"
        
        # Path to save the output CSV file
        output_csv_file = "output_base64.csv"
        
        # Target character limit for the base64 string
        character_limit = 32767
        
        # Function to compress the image
        def compress_image(image, quality=85):
            buffer = BytesIO()
            image.save(buffer, format="PNG", optimize=True, quality=quality)
            return buffer.getvalue()
        
        # Open the original image
        with Image.open(image_path) as img:
            # Resize the image iteratively until the base64 string is below the limit
            quality = 85  # Start with high quality
            while True:
                # Compress and encode the image
                compressed_data = compress_image(img, quality)
                base64_encoded_image = base64.b64encode(compressed_data).decode("utf-8")
                
                # Check the character length
                if len(base64_encoded_image) <= character_limit:
                    break
                
                # Reduce quality for the next iteration
                quality -= 5
                if quality < -50:
                    raise ValueError("Cannot compress the image further while keeping it below the character limit.")
            
            print(f"Final base64 string length: {len(base64_encoded_image)}")
        
        # Write the base64 string to a CSV file
        with open(output_csv_file, "w", newline="", encoding="utf-8") as csv_file:
            writer = csv.writer(csv_file)
            
            # Optional header row
            writer.writerow(["Description", "Base64 Image"])
            
            # Write the base64 string
            writer.writerow(["Compressed Image", base64_encoded_image])
        
        print(f"Base64-encoded image saved to {output_csv_file}")


​​​ Creating a Base64 encoded image from a folder containing Images

This Python script scans a folder for .png image files, compresses and resizes them as needed, and converts them into Base64-encoded JPEG strings. It then saves the results into a CSV file, which can be used in tools like Power BI.

Key Features:
Automatic Folder Scanning: No need to list filenames manually,  the script finds all .png files in the specified folder.
Image Conversion: Each PNG image is converted to JPEG (to reduce size and remove transparency). Compression & Resizing: Images are automatically compressed and resized to ensure the Base64 string stays under Power BI’s 32,767-character limit.
CSV Output: The output CSV file contains two columns: the original filename and its Base64-encoded image string.


import base64
from PIL import Image
from io import BytesIO
import csv
import os

# Folder containing PNG images
image_folder = "images"  # Change this to your folder path
output_csv_file = "output_base64_jpeg.csv"

# Base64 character limit
character_limit = 32767

# Compress and resize JPEG for stronger compression (no transparency)
def compress_image(image, quality=85, resize_factor=1.0):
    buffer = BytesIO()

    # JPEG requires RGB
    if image.mode != "RGB":
        image = image.convert("RGB")

    # Resize if needed
    if resize_factor < 1.0:
        new_size = (
            int(image.width * resize_factor),
            int(image.height * resize_factor)
        )
        image = image.resize(new_size, Image.Resampling.LANCZOS)

    image.save(buffer, format="JPEG", quality=quality, optimize=True)
    return buffer.getvalue()

# Scan folder for PNG images
image_paths = [
    os.path.join(image_folder, file)
    for file in os.listdir(image_folder)
    if file.lower().endswith(".png")
]

# Write base64 strings to CSV
with open(output_csv_file, "w", newline="", encoding="utf-8") as csv_file:
    writer = csv.writer(csv_file)
    writer.writerow(["Filename", "Base64 Image"])  # Header

    for image_path in image_paths:
        try:
            with Image.open(image_path) as img:
                resize_factor = 1.0
                quality = 85

                while True:
                    compressed_data = compress_image(img, quality=quality, resize_factor=resize_factor)
                    base64_encoded_image = base64.b64encode(compressed_data).decode("utf-8")

                    if len(base64_encoded_image) <= character_limit:
                        break

                    if quality > 40:
                        quality -= 5
                    else:
                        resize_factor -= 0.05

                    if resize_factor < 0.1:
                        raise ValueError(f"Cannot compress image {image_path} below character limit.")

                writer.writerow([os.path.basename(image_path), base64_encoded_image])
                print(f"{image_path}: success (length = {len(base64_encoded_image)})")

        except Exception as e:
            print(f"Error processing {image_path}: {e}")
