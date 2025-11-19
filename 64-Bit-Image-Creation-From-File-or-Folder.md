# Converting Images to Base64 for Power BI

This guide explains how to generate Base64-encoded image strings from:

- A **single image file**, or  
- An **entire folder of images**

Both examples use Python and automatically compress images so the final Base64 string stays under the **32,767-character limit**, which helps ensure compatibility with Power BI text fields.

> 💡 **Tip**  
> You’ll need Python, plus the [`Pillow`](https://pillow.readthedocs.io/) library installed (`pip install pillow`).

---

## 🔹 Option 1 — Create a Base64 String from a Single Image

The script below:

1. Loads a PNG image.
2. Iteratively compresses it until the Base64 string is under the character limit.
3. Writes the final Base64 string to a CSV file (with an optional header).

Make sure to update the `image_path` and `output_csv_file` values before running.

```python
import base64
from PIL import Image
from io import BytesIO
import csv  # Ensure the csv module is imported

# Path to your PNG image
image_path = "Example.png"

# Path to save the output CSV file
output_csv_file = "output_base64.csv"

# Target character limit for the Base64 string
character_limit = 32767

# Function to compress the image
def compress_image(image, quality=85):
    buffer = BytesIO()
    # Note: PNG ignores 'quality' but will use 'optimize'
    image.save(buffer, format="PNG", optimize=True)
    return buffer.getvalue()

# Open the original image
with Image.open(image_path) as img:
    quality = 85  # Placeholder if you later adapt this for JPEG

    while True:
        # Compress and encode the image
        compressed_data = compress_image(img, quality)
        base64_encoded_image = base64.b64encode(compressed_data).decode("utf-8")

        # Check the character length
        if len(base64_encoded_image) <= character_limit:
            break

        # Reduce quality for the next iteration (if using JPEG)
        quality -= 5
        if quality < -50:
            raise ValueError(
                "Cannot compress the image further while keeping it below the character limit."
            )

    print(f"Final Base64 string length: {len(base64_encoded_image)}")

# Write the Base64 string to a CSV file
with open(output_csv_file, "w", newline="", encoding="utf-8") as csv_file:
    writer = csv.writer(csv_file)

    # Optional header row
    writer.writerow(["Description", "Base64 Image"])

    # Write the Base64 string
    writer.writerow(["Compressed Image", base64_encoded_image])

print(f"Base64-encoded image saved to {output_csv_file}")
```


✅ Output format
The CSV will contain:

Description — a label for the image

Base64 Image — the encoded image string (without any data:image/...;base64, prefix)

🔹 Option 2 — Create Base64 Strings from All Images in a Folder

This script scans a folder, processes all .png files, and exports a CSV with one row per image.

What it does:

Automatic folder scanning
Finds all .png files in the specified folder — no need to list filenames manually.

Image conversion
Converts each PNG to JPEG (to reduce size and remove transparency).

Compression and resizing
Automatically compresses and resizes until the Base64 string is below the 32,767-character limit.

CSV output
Saves a CSV file with two columns:

Filename

Base64 Image

Update image_folder and output_csv_file to match your environment:
```python
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

# Write Base64 strings to CSV
with open(output_csv_file, "w", newline="", encoding="utf-8") as csv_file:
    writer = csv.writer(csv_file)
    writer.writerow(["Filename", "Base64 Image"])  # Header

    for image_path in image_paths:
        try:
            with Image.open(image_path) as img:
                resize_factor = 1.0
                quality = 85

                while True:
                    compressed_data = compress_image(
                        img,
                        quality=quality,
                        resize_factor=resize_factor
                    )
                    base64_encoded_image = base64.b64encode(
                        compressed_data
                    ).decode("utf-8")

                    if len(base64_encoded_image) <= character_limit:
                        break

                    # First reduce quality, then start resizing
                    if quality > 40:
                        quality -= 5
                    else:
                        resize_factor -= 0.05

                    if resize_factor < 0.1:
                        raise ValueError(
                            f"Cannot compress image {image_path} below character limit."
                        )

                writer.writerow([os.path.basename(image_path), base64_encoded_image])
                print(f"{image_path}: success (length = {len(base64_encoded_image)})")

        except Exception as e:
            print(f"Error processing {image_path}: {e}")
```

📄 Using in Power BI
Once generated, you can load the CSV into Power BI and use the Base64 column in measures or columns, depending on your visual’s requirements. Make sure the field is treated as text and that your visual expects a raw Base64 string (without the data:image/jpeg;base64, prefix).
