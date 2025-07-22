<script>
  import { createEventDispatcher, onMount } from "svelte";

  export let selectedMahasiswa = null;
  export let isEditMode = false;

  let nama = "";
  let nim = "";
  let jurusan = "";
  let ipk = "";
  let ipkError = "";

  // Set nilai hanya saat masuk mode edit atau selectedMahasiswa berubah
  let prevId = null;
  $: if (isEditMode && selectedMahasiswa && selectedMahasiswa.id !== prevId) {
    nama = selectedMahasiswa.nama;
    nim = selectedMahasiswa.nim;
    jurusan = selectedMahasiswa.jurusan;
    ipk = selectedMahasiswa.ipk;
    prevId = selectedMahasiswa.id;
  }

  const dispatch = createEventDispatcher();

  const handleSubmit = (e) => {
    e.preventDefault();
    ipkError = "";
    if (parseFloat(ipk) > 4) {
      ipkError = "IPK tidak boleh lebih dari 4.00";
      return;
    }
    dispatch("addMahasiswa", {
      id: isEditMode && selectedMahasiswa ? selectedMahasiswa.id : Date.now(),
      nama,
      nim,
      jurusan,
      ipk: parseFloat(ipk),
    });
    // Reset form opsional
    if (!isEditMode) {
      nama = "";
      nim = "";
      jurusan = "";
      ipk = "";
    }
  };

  const jurusanOptions = [
    "Sistem Informasi",
    "Teknik Informatika",
    "Rekayasa Perangkat Lunak",
    "Data Science",
    "Manajemen",
    "Akuntansi"
  ];
</script>

<h3 class="form-title">
  <span class="iconify" data-icon={isEditMode ? 'mdi:account-edit' : 'mdi:account-plus'} style="vertical-align: middle; margin-right: 0.5rem;"></span>
  {isEditMode ? 'Edit Mahasiswa' : 'Tambah Mahasiswa'}
</h3>
<form on:submit={handleSubmit} class="form-modern">
  <div class="form-group">
    <label for="nama">Nama</label>
    <input id="nama" type="text" placeholder="Nama" bind:value={nama} required />
  </div>
  <div class="form-group">
    <label for="nim">NIM</label>
    <input id="nim" type="number" placeholder="NIM" bind:value={nim} required />
  </div>
  <div class="form-group">
    <label for="jurusan">Jurusan</label>
    <select id="jurusan" bind:value={jurusan} required>
      <option value="" disabled selected>Pilih Jurusan</option>
      {#each jurusanOptions as option}
        <option value={option}>{option}</option>
      {/each}
    </select>
  </div>
  <div class="form-group">
    <label for="ipk">IPK</label>
    <input id="ipk" type="number" step="0.01" min="0" max="4" placeholder="IPK" bind:value={ipk} required />
    {#if ipkError}
      <span class="error">{ipkError}</span>
    {/if}
  </div>
  <button type="submit" class="submitBtn">
    <span class="iconify" data-icon={isEditMode ? 'mdi:content-save-edit' : 'mdi:plus-circle'} style="vertical-align: middle; margin-right: 0.4rem;"></span>
    {isEditMode ? 'Simpan Perubahan' : 'Tambah'}
  </button>
</form>

<style>
  .form-title {
    display: flex;
    align-items: center;
    font-size: 1.25rem;
    font-weight: 600;
    margin-bottom: 1.2rem;
    color: #2563eb;
    justify-content: center;
  }
  .form-modern {
    display: flex;
    flex-direction: column;
    gap: 0.7rem;
    padding: 0.5rem 0.2rem 0.2rem 0.2rem;
  }
  .form-group {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    gap: 0.2rem;
  }
  input, select {
    width: 100%;
    font-size: 1.05rem;
    border-radius: 8px;
    border: 1.5px solid #e5e7eb;
    padding: 0.7em 1em;
    background: #f9fafb;
    transition: border 0.2s, box-shadow 0.2s;
  }
  input:focus, select:focus {
    border-color: #2563eb;
    box-shadow: 0 0 0 2px #2563eb22;
    outline: none;
  }
  .error {
    color: #ef4444;
    font-size: 0.97em;
    margin-top: 0.1em;
    font-weight: 500;
  }
  .submitBtn {
    display: flex;
    align-items: center;
    justify-content: center;
    background: linear-gradient(90deg, #2563eb 60%, #6366f1 100%);
    color: #fff;
    font-weight: 600;
    font-size: 1.08rem;
    border: none;
    border-radius: 8px;
    padding: 0.7em 1.2em;
    margin-top: 0.5rem;
    box-shadow: 0 2px 8px 0 #2563eb11;
    cursor: pointer;
    transition: background 0.2s, box-shadow 0.2s;
  }
  .submitBtn:hover {
    background: linear-gradient(90deg, #1d4ed8 60%, #6366f1 100%);
  }
</style>
