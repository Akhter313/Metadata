# Metadata
from PIL import Image
from PIL.ExifTags import TAGS
import sys

def get_exif_data(image_path):
    try:
        image = Image.open(image_path)
        exif_data = image._getexif()
        if not exif_data:
            print("No EXIF metadata found.")
            return None

        metadata = {}
        for tag_id, value in exif_data.items():
            tag = TAGS.get(tag_id, tag_id)
            metadata[tag] = value
        return metadata

    except Exception as e:
        print(f"Error: {e}")
        return None

def save_report(metadata, output_file):
    with open(output_file, 'w') as f:
        for tag, value in metadata.items():
            f.write(f"{tag}: {value}\n")

if __name__ == "__main__":
    if len(sys.argv) != 2:
        print("Usage: python image_metadata_extractor.py <image_path>")
        sys.exit(1)

    image_path = sys.argv[1]
    metadata = get_exif_data(image_path)

    if metadata:
        for tag, value in metadata.items():
            print(f"{tag}: {value}")
        report_file = "metadata_report.txt"
        save_report(metadata, report_file)
        print(f"\nMetadata saved to {report_file}")
