# 🚀 Integrasi LLMs dengan Watsonx.ai

Dokumentasi ini menjelaskan langkah-langkah untuk mengintegrasikan Watsonx.ai LLM (Large Language Models) ke dalam aplikasi menggunakan REST API atau Python SDK.

---

## 📋 Daftar Isi

1. [Pendahuluan](#-pendahuluan)
2. [Langkah-Langkah Utama](#-langkah-langkah-utama)
   - [Uji Prompt di Watsonx.ai](#a-uji-prompt-di-watsonxai)
   - [Mendapatkan Token Autentikasi](#b-mendapatkan-token-autentikasi)
3. [Integrasi REST API](#-integrasi-rest-api)
4. [Menggunakan Notebook Python](#-menggunakan-notebook-python)
5. [Membangun UI dengan Streamlit](#-membangun-ui-dengan-streamlit)
6. [Catatan Penting](#-catatan-penting)
7. [Referensi](#-referensi)

---

## 📝 Pendahuluan

Watsonx.ai menyediakan berbagai model generatif yang dapat digunakan untuk aplikasi bisnis. Dalam panduan ini, kita akan:
- Menguji prompt menggunakan Watsonx.ai Prompt Lab.
- Mengintegrasikan model menggunakan REST API dan Python SDK.
- Membangun UI sederhana menggunakan Streamlit.

---

## 🔧 Langkah-Langkah Utama

### **a. Uji Prompt di Watsonx.ai**
1. **Masuk ke Prompt Lab**
   - Buka Prompt Lab di Watsonx.ai.
   - Pilih salah satu prompt atau gunakan contoh berikut:
     ```plaintext
     Please provide top 5 bullet points in the review provided in '''.

     Review:
     '''I had 2 problems with my experience with my refinance. ...'''
     ```

2. **Atur Parameter Model**
   - Gunakan model seperti `flan-ul2`.
   - Set parameter decoding sebagai:
     - `decoding_method = greedy`
     - `min tokens = 50`
     - `max tokens = 300`
   - Tambahkan stop sequence (`.`).

3. **Generate Hasil**
   - Klik **Generate** untuk melihat output.
   - Simpan kode REST API menggunakan tombol **View Code**.

---

### **b. Mendapatkan Token Autentikasi**
1. **Buat API Key IBM Cloud**
   - Buat API Key sesuai [Panduan IBM Cloud](https://cloud.ibm.com/docs/account?topic=account-userapikey).
   - Simpan key dalam file `.env` dengan format:
     ```
     API_KEY=your_api_key
     PROJECT_ID=your_project_id
     ```

2. **Dapatkan Project ID**
   - Login ke Watsonx.ai dan salin **Project ID**.

---

## 🌐 Integrasi REST API

1. **Contoh Skrip REST API**
   Gunakan skrip berikut untuk mengakses model:
   ```bash
   curl 'https://us-south.ml.cloud.ibm.com/ml/v1-beta/generation/text?...' \
   -H 'Content-Type: application/json' \
   -H 'Authorization: Bearer YOUR_ACCESS_TOKEN' \
   -d '{
     "input": "Please provide top 5 bullet points...",
     "parameters": {
       "decoding_method": "greedy",
       "max_new_tokens": 300,
       "min_new_tokens": 50,
       "stop_sequences": [ "." ]
     },
     "model_id": "google/flan-ul2",
     "project_id": "YOUR_PROJECT_ID"
   }'
## 💻 Menggunakan Notebook Python

1. **Buka Notebook**
   - Masuk ke Watsonx.ai dan pilih **Work with data and models in Python or R notebooks**.
   - Unggah file notebook `TestLLM.ipynb` ke Watsonx.ai.

2. **Konfigurasi Notebook**
   - Pastikan memilih runtime **Python 3.10 XS** (2 vCPU, 8 GB RAM).
   - File notebook harus memiliki ekstensi `.ipynb`.

3. **Instal Dependensi**
   - Sebelum menjalankan kode, pastikan semua dependensi terinstal.
   - Jalankan perintah berikut di terminal:
     ```bash
     python3 -m pip install -r requirements.txt
     ```

4. **Struktur Kode Python**  
   File Python `demo_wml_api.py` menyediakan berbagai fungsi untuk berinteraksi dengan Watsonx.ai. Berikut adalah fungsi utamanya:

   - **`get_credentials()`**  
     Membaca API key dan Project ID dari file `.env` untuk otentikasi.
     ```python
     def get_credentials():
         with open(".env", "r") as f:
             credentials = f.read().splitlines()
         return credentials
     ```

   - **`get_model()`**  
     Membuat objek model LLM dengan parameter tertentu.
     ```python
     def get_model():
         model = {
             "model_id": "google/flan-ul2",
             "project_id": "YOUR_PROJECT_ID"
         }
         return model
     ```

   - **`invoke_with_REST()`**  
     Mengirim permintaan ke Watsonx.ai menggunakan REST API.
     ```python
     import requests

     def invoke_with_REST(input_text):
         url = "https://us-south.ml.cloud.ibm.com/ml/v1-beta/generation/text"
         headers = {
             "Content-Type": "application/json",
             "Authorization": f"Bearer YOUR_ACCESS_TOKEN"
         }
         payload = {
             "input": input_text,
             "parameters": {
                 "decoding_method": "greedy",
                 "max_new_tokens": 300,
                 "min_new_tokens": 50,
                 "stop_sequences": ["."]
             },
             "model_id": "google/flan-ul2",
             "project_id": "YOUR_PROJECT_ID"
         }
         response = requests.post(url, headers=headers, json=payload)
         return response.json()
     ```

5. **Menjalankan Notebook**  
   Jalankan semua sel di notebook untuk:
   - Membaca kredensial API.
   - Memanggil model Watsonx.ai.
   - Menghasilkan respons berdasarkan prompt yang diberikan.

---

Sekarang bagian ini siap digunakan untuk dimasukkan ke dalam `README.md` Anda. Jika Anda memerlukan penjelasan tambahan, beri tahu saya! 😊

