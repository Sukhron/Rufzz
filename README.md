import os
import zipfile
import shutil
from pathlib import Path

# === GANTI BAGIAN INI SESUAI FILE KAMU ===
zip_source = Path("hadiah-ulangtahun-final.zip")  # ZIP asli kamu
readme_file = Path("README_Victoria.md")          # README romantis
banner_file = Path("A_digital_graphic_design_image_features_a_birthday.png")  # Banner
output_zip = Path("hadiah-ulangtahun-final-with-readme.zip")  # Hasil akhir

# === EKSTRAK FILE ZIP ASLI ===
extract_dir = Path("temp_ulangtahun")
if extract_dir.exists():
    shutil.rmtree(extract_dir)
extract_dir.mkdir(parents=True)

with zipfile.ZipFile(zip_source, "r") as zip_ref:
    zip_ref.extractall(extract_dir)

# === SALIN README & BANNER KE ROOT ===
shutil.copy(readme_file, extract_dir / "README.md")
shutil.copy(banner_file, extract_dir / banner_file.name)

# === BUAT ZIP BARU DENGAN FILE TAMBAHAN ===
if output_zip.exists():
    output_zip.unlink()

with zipfile.ZipFile(output_zip, "w", zipfile.ZIP_DEFLATED) as zip_ref:
    for folder, _, files in os.walk(extract_dir):
        for file in files:
            file_path = Path(folder) / file
            zip_ref.write(file_path, file_path.relative_to(extract_dir))

print(f"✅ Paket final berhasil dibuat: {output_zip}")

