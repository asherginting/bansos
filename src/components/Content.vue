<template>
  <div class="card">
    <div class="card-header">
      <h2 class="text-center" style="font-weight: bold;">Form Penerima Manfaat Bansos</h2>
    </div>
    <div class="card-body">
      <form @submit.prevent="submitForm">
        <div class="form-group">
          <label for="nama">Nama</label>
          <input type="text" id="nama" v-model="formData.nama" class="form-control" required />
        </div>
        <div class="form-group">
          <label for="nik">NIK</label>
          <input type="number" id="nik" v-model="formData.nik" class="form-control" required />
        </div>
        <div class="form-group">
          <label for="noKartuKeluarga">Nomor Kartu Keluarga</label>
          <input type="text" id="noKartuKeluarga" v-model="formData.noKartuKeluarga" class="form-control" required />
        </div>
        <div class="form-group">
          <label for="fotoKTP">Foto KTP</label>
          <input type="file" id="fotoKTP" accept="image/jpeg, image/png, image/bmp" @change="validateFile" class="form-control" required style="color: black;" />
          <small class="text-muted" style="color: black;">Maksimal 2MB, format JPG/JPEG/PNG/BMP</small>
        </div>
        <div class="form-group">
          <label for="fotoKK">Foto Kartu Keluarga</label>
          <input type="file" id="fotoKK" accept="image/jpeg, image/png, image/bmp" @change="validateFile" class="form-control" required style="color: black;" />
          <small class="text-muted" style="color: black;">Maksimal 2MB, format JPG/JPEG/PNG/BMP</small>
        </div>
        <div class="form-group">
          <label for="umur">Umur</label>
          <input type="number" id="umur" v-model="formData.umur" class="form-control" required />
          <small class="text-muted" style="color: black;">Berumur lebih dari atau sama dengan 25 tahun</small>
        </div>
        <div class="form-group">
          <label for="jenisKelamin">Jenis Kelamin</label>
          <select id="jenisKelamin" v-model="formData.jenisKelamin" class="form-control" required>
            <option value="Laki-laki">Laki-laki</option>
            <option value="Perempuan">Perempuan</option>
          </select>
        </div>
        <div class="form-group">
          <label for="provinsi">Provinsi</label>
          <select id="provinsi" v-model="formData.provinsi" class="form-control" required @change="loadKabKota">
            <option value="">Pilih Provinsi</option>
            <option v-for="provinsi in provinsiList" :key="provinsi.id" :value="provinsi.id">{{ provinsi.name }}</option>
          </select>
        </div>
        <div class="form-group">
          <label for="kabKota">Kab/Kota</label>
          <select id="kabKota" v-model="formData.kabKota" class="form-control" required @change="loadKecamatan">
            <option value="">Pilih Kab/Kota</option>
            <option v-for="kabKota in kabKotaList" :key="kabKota.id" :value="kabKota.id">{{ kabKota.name }}</option>
          </select>
        </div>
        <div class="form-group">
          <label for="kecamatan">Kecamatan</label>
          <select id="kecamatan" v-model="formData.kecamatan" class="form-control" required @change="loadKelurahanDesa">
            <option value="">Pilih Kecamatan</option>
            <option v-for="kecamatan in kecamatanList" :key="kecamatan.id" :value="kecamatan.id">{{ kecamatan.name }}</option>
          </select>
        </div>
        <div class="form-group">
          <label for="kelurahanDesa">Kelurahan/Desa</label>
          <select id="kelurahanDesa" v-model="formData.kelurahanDesa" class="form-control" required>
            <option value="">Pilih Kelurahan/Desa</option>
            <option v-for="kelurahanDesa in kelurahanDesaList" :key="kelurahanDesa.id" :value="kelurahanDesa.id">{{ kelurahanDesa.name }}</option>
          </select>
        </div>
        <div class="form-group">
          <label for="alamat">Alamat</label>
          <input type="text" id="alamat" v-model="formData.alamat" class="form-control" required maxlength="255" />
        </div>
        <div class="form-group">
          <label for="rt">RT</label>
          <input type="number" id="rt" v-model="formData.rt" class="form-control" />
        </div>
        <div class="form-group">
          <label for="rw">RW</label>
          <input type="number" id="rw" v-model="formData.rw" class="form-control" />
        </div>
        <div class="form-group">
          <label for="penghasilanSebelumPandemi">Penghasilan sebelum pandemi</label>
          <input type="number" id="penghasilanSebelumPandemi" v-model="formData.penghasilanSebelumPandemi" class="form-control" required />
        </div>
        <div class="form-group">
          <label for="penghasilanSetelahPandemi">Penghasilan setelah pandemi</label>
          <input type="number" id="penghasilanSetelahPandemi" v-model="formData.penghasilanSetelahPandemi" class="form-control" required />
        </div>
        <div class="form-group">
          <label for="alasanBantuan">Alasan membutuhkan bantuan</label>
          <select id="alasanBantuan" v-model="formData.alasanBantuan" class="form-control" required>
            <option value="Kehilangan pekerjaan">Kehilangan pekerjaan</option>
            <option value="Kepala keluarga terdampak atau korban Covid-19">Kepala keluarga terdampak atau korban Covid-19</option>
            <option value="Tergolong fakir/miskin semenjak sebelum Covid-19">Tergolong fakir/miskin semenjak sebelum Covid-19</option>
            <option value="Lainnya">Lainnya</option>
          </select>
          <input v-if="formData.alasanBantuan === 'Lainnya'" type="text" v-model="formData.alasanLainnya" class="form-control mt-2" placeholder="Lainnya: ..." required style="margin-top: 15px;" />
        </div>
        <div class="form-check">
          <input type="checkbox" id="persetujuan" v-model="formData.persetujuan" class="form-check-input" required />
          <label class="form-check-label" for="persetujuan" style="color: black; font-size: 13px; margin-left: 5px;">Saya menyatakan bahwa data yang diisikan adalah benar dan siap mempertanggungjawabkan apabila ditemukan ketidaksesuaian dalam data tersebut.</label>
        </div>
        <button type="submit" class="btn btn-primary" style="margin-top: 10px;">
          {{ submitting ? 'Loading...' : 'Submit' }}
        </button>
      </form>

      <div class="modal-background" v-if="showModal">
        <div class="modal-container">
          <div class="modal">
            <div class="custom-modal-body">
              <h3 style="color: black; font-weight: bold; text-align: center;">Data Penerima Manfaat Bansos</h3>
              <hr style="margin-bottom: 20px;"/>
              <div class="data-row">
                <span class="data-label">Nama:</span>
                <span class="data-value">{{ modalData.nama }}</span>
              </div>
              <div class="data-row">
                <span class="data-label">NIK:</span>
                <span class="data-value">{{ modalData.nik }}</span>
              </div>
              <div class="data-row">
                <span class="data-label">Nomor Kartu Keluarga:</span>
                <span class="data-value">{{ modalData.noKartuKeluarga }}</span>
              </div>
              <div class="data-row">
                <span class="data-label">Foto KTP:</span>
                <span class="data-value">-</span>
              </div>
              <div class="data-row">
                <span class="data-label">Foto KK:</span>
                <span class="data-value">-</span>
              </div>
              <div class="data-row">
                <span class="data-label">Umur:</span>
                <span class="data-value">{{ modalData.umur }}</span>
              </div>
              <div class="data-row">
                <span class="data-label">Jenis Kelamin:</span>
                <span class="data-value">{{ modalData.jenisKelamin }}</span>
              </div>
              <div class="data-row">
                <span class="data-label">Provinsi:</span>
                <span class="data-value">{{ getNamaProvinsi(modalData.provinsi) }}</span>
              </div>
              <div class="data-row">
                <span class="data-label">Kab/Kota</span>
                <span class="data-value">{{ getNamaKabKota(modalData.kabKota) }}</span>
              </div>
              <div class="data-row">
                <span class="data-label">Kecamatan</span>
                <span class="data-value">{{ getNamaKecamatan(modalData.kecamatan) }}</span>
              </div>
              <div class="data-row">
                <span class="data-label">Kelurahan/Desa</span>
                <span class="data-value">{{ getNamaKelurahanDesa(modalData.kelurahanDesa) }}</span>
              </div>
              <div class="data-row">
                <span class="data-label">Alamat:</span>
                <span class="data-value">{{ modalData.alamat }}</span>
              </div>
              <div class="data-row">
                <span class="data-label">RT:</span>
                <span class="data-value">{{ modalData.rt}}</span>
              </div>
              <div class="data-row">
                <span class="data-label">RW:</span>
                <span class="data-value">{{ modalData.rw}}</span>
              </div>
              <div class="data-row">
                <span class="data-label">Penghasilan sebelum pandemi: </span>
                <span class="data-value">{{ modalData.penghasilanSebelumPandemi}}</span>
              </div>
              <div class="data-row">
                <span class="data-label">Penghasilan setelah pandemi: </span>
                <span class="data-value">{{ modalData.penghasilanSetelahPandemi}}</span>
              </div>
              <div class="data-row">
                <span class="data-label">Alasan membutuhkan bantuan:</span>
                <span class="data-value">{{ modalData.alasanBantuan || modalData.alasanLainnya }}</span>
              </div>
            </div>
            <div class="custom-modal-footer">
              <button type="button" class="custom-btn btn-secondary" @click="closeModal">Close</button>
            </div>
          </div>
        </div>
      </div>

    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      formData: {
        nama: '',
        nik: null,
        noKartuKeluarga: null,
        fotoKTP: null,
        fotoKK: null,
        umur: null,
        jenisKelamin: '',
        alamat: '',
        rt: '',
        rw: '',
        penghasilanSebelumPandemi: null,
        penghasilanSetelahPandemi: null,
        alasanBantuan: '',
        alasanLainnya: '',
        persetujuan: false,
      },
      provinsiList: [],
      kabKotaList: [],
      kecamatanList: [], 
      kelurahanDesaList: [], 
      submitting: false,
      showModal: false,
      modalData: {
        nama: '',
        nik: null,
        noKartuKeluarga: '',
        fotoKTP: null,
        fotoKK: null,
        umur: null,
        jenisKelamin: '',
        alamat: '',
        rt: null,
        rw: null,
        penghasilanSebelumPandemi: null,
        penghasilanSetelahPandemi: null,
        alasanBantuan: '',
        alasanLainnya: '',
        persetujuan: false,
        provinsi: '', 
        kabKota: '',
        kecamatan: '',
        kelurahanDesa: '',
      },
    };
  },
  methods: {
    submitForm() {
      this.submitting = true;
      setTimeout(() => {
        const success = Math.random() < 0.5; 
        this.submitting = false; 

        if (success) {
          console.log("Data berhasil dikirim:", this.formData);
          this.showModal = true;

          this.modalData = { ...this.formData };

          this.formData = {
            nama: '',
            nik: null,
            noKartuKeluarga: null,
            fotoKTP: null,
            fotoKK: null,
            umur: null,
            jenisKelamin: '',
            alamat: '',
            rt: '',
            rw: '',
            penghasilanSebelumPandemi: null,
            penghasilanSetelahPandemi: null,
            alasanBantuan: '',
            alasanLainnya: '',
            persetujuan: false,
          };

        } else {
          console.error("Gagal mengirim data.");
        }
      }, 1500);
    },
      closeModal() {
      this.showModal = false;
    },

    getNamaProvinsi(id) {
      const provinsi = this.provinsiList.find(item => item.id === id);
      return provinsi ? provinsi.name : '';
    },

    getNamaKabKota(id) {
      const kabKota = this.kabKotaList.find(item => item.id === id);
      return kabKota ? kabKota.name : '';
    },

    getNamaKecamatan(id) {
      const kecamatan = this.kecamatanList.find(item => item.id === id);
      return kecamatan ? kecamatan.name : '';
    },

    getNamaKelurahanDesa(id) {
      const kelurahanDesa = this.kelurahanDesaList.find(item => item.id === id);
      return kelurahanDesa ? kelurahanDesa.name : '';
    },

    validateFile(event) {
      const file = event.target.files[0];
      if (file && file.size > 2 * 1024 * 1024) {
        alert('Ukuran file melebihi 2MB.');
        event.target.value = '';
      }
    },
    loadProvinsi() {
      axios.get(`https://www.emsifa.com/api-wilayah-indonesia/api/provinces.json`)
        .then(response => {
          this.provinsiList = response.data;
        })
        .catch(error => {
          console.error('Error fetching provinsi data:', error);
        });
    },
    loadKabKota() {
      if (this.formData.provinsi) {
        axios.get(`https://www.emsifa.com/api-wilayah-indonesia/api/regencies/${this.formData.provinsi}.json`)
          .then(response => {
            this.kabKotaList = response.data;
          })
          .catch(error => {
            console.error('Error fetching kabupaten/kota data:', error);
          });
      }
    },
    loadKecamatan() {
      if (this.formData.kabKota) {
        axios.get(`https://www.emsifa.com/api-wilayah-indonesia/api/districts/${this.formData.kabKota}.json`)
          .then(response => {
            this.kecamatanList = response.data;
          })
          .catch(error => {
            console.error('Error fetching kecamatan data:', error);
          });
      }
    },
    loadKelurahanDesa() {
      if (this.formData.kecamatan) {
        axios.get(`https://www.emsifa.com/api-wilayah-indonesia/api/villages/${this.formData.kecamatan}.json`)
          .then(response => {
            this.kelurahanDesaList = response.data;
          })
          .catch(error => {
            console.error('Error fetching kelurahan/desa data:', error);
          });
      }
    },
  },
  mounted() {
    this.loadProvinsi(); 
  },
};
</script>

<style>
.form-content {
  margin-top: 220px;
}
.card {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
  margin-top: 110px;
  background-color: #ffff;
}
.card-header {
  color: #000;
  text-align: center;
  padding: 10px;
}
.card-body {
  padding: 20px;
}
.form-group {
  margin-bottom: 20px;
}
.form-group label {
  color: #000;
  font-weight: bold;
  font-size: 13px;
}
.form-label {
  font-weight: bold;
  color: #000;
}
.form-control {
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
}
.small-text {
  font-size: 12px;
  color: #000;
}
.checkbox-label {
  font-size: 14px;
  font-weight: normal;
  color: #000;
}
.checkbox-input {
  margin-right: 5px;
}
.btn-primary {
  background-color: #007bff;
  color: #fff;
  border: none;
  padding: 10px 20px;
  cursor: pointer;
  border-radius: 5px;
  transition: background-color 0.3s;
}
.btn-primary:hover {
  background-color: #0056b3;
}
.modal-background {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.7);
  display: flex;
  justify-content: center;
  align-items: center;
  overflow: hidden;
}
.modal-container {
  background: #fff;
  padding: 20px;
  border-radius: 5px;
  height: 550px;
  width: 600px;
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.5);
}
.modal {
  width: 100%;
  max-width: 600px;
}
.data-row {
  display: flex;
  justify-content: space-between;
}
.data-label {
  font-weight: bold;
  color: #000;
}
.data-value {
  color: #333;
}
.custom-modal-footer{
  text-align: center;
  margin-top: 20px;
}
</style>

