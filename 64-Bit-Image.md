The following script utilizes the base64 library and the PIL package to handle image processing. It begins by importing necessary modules and specifying paths for both the input PNG image and the output CSV file. The script sets a character limit for the base64 encoded string and defines a function to compress the image. The original image is opened, and a loop is initiated to iteratively compress and encode the image until the base64 string meets the character limit. The script then writes the resulting base64 string to a specified CSV file, including an optional header row.

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
        
