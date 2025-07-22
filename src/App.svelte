<!-- <script>
	let firstName = 'Moh';
	let lastName = 'Sultoni';
	let jurusan = 'Sistem Informasi';

	// reactive value
	$: fullName = `${firstName} ${lastName}`;

	// reactive statement
	$: {
		console.log(fullName);
		console.log(jurusan);
	}

	const handleClick = () => {
		jurusan = 'Teknologi Informasi';
	}

	const handleInput = (e) => {
		jurusan = e.target.value;	
	}
</script> -->

<!-- <main>
	<h1>Hello {fullName}!</h1>
	<h2>Jurusan: {jurusan}</h2>
	<p>Lorem, ipsum dolor sit amet consectetur adipisicing elit. Eaque, aut porro asperiores hic expedita repellendus quae, ut consequuntur, facilis corporis sequi incidunt laudantium dolore deserunt molestiae est ea quaerat quis?</p>
	<button on:click={handleClick}>Update jurusan</button>
	<input type="text" bind:value={firstName}>
	<input type="text" bind:value={lastName}>

	two way binding
	 <input type="text" bind:value={jurusan}> 
</main> -->

<script>
  import Modal from "./Modal.svelte";
  import Form from "./Form.svelte";
  import Alert from "./Alert.svelte";
  import { tick } from 'svelte';

  let openModal = false;
  let showDetailModal = false;
  let showFormModal = false;
  let selectedMahasiswa = null;
  let isEditMode = false;
  let showConfirmDelete = false;
  let mahasiswaToDelete = null;
  let alertMessage = '';
  let alertType = 'success';
  let alertVisible = false;

  async function showAlert(msg, type = 'success') {
    alertVisible = false;
    await tick();
    alertMessage = msg;
    alertType = type;
    alertVisible = true;
  }

  const showDetail = (mhs) => {
    selectedMahasiswa = mhs;
    showDetailModal = true;
  };

  const closeDetailModal = () => {
    showDetailModal = false;
    selectedMahasiswa = null;
  };

  const openAddForm = () => {
    showFormModal = true;
    isEditMode = false;
    selectedMahasiswa = null;
  };

  const openEditForm = (mhs) => {
    selectedMahasiswa = { ...mhs };
    showFormModal = true;
    isEditMode = true;
  };

  const closeFormModal = () => {
    showFormModal = false;
    selectedMahasiswa = null;
    isEditMode = false;
  };

  let mahasiswa = [
    {
      id: 1,
      nama: "Moh Sultoni",
      nim: 123456,
      jurusan: "Sistem Informasi",
      ipk: 2.8,
    },
    {
      id: 2,
      nama: "Budi",
      nim: 123457,
      jurusan: "Sistem Informasi",
      ipk: 2.1,
    },
    {
      id: 3,
      nama: "Andi",
      nim: 123458,
      jurusan: "Sistem Informasi",
      ipk: 3.6,
    },
  ];

  const handleDelete = (id) => {
    showConfirmDelete = true;
    mahasiswaToDelete = id;
  };

  const confirmDelete = () => {
    mahasiswa = mahasiswa.filter((mhs) => mhs.id !== mahasiswaToDelete);
    showConfirmDelete = false;
    mahasiswaToDelete = null;
    showAlert('Data mahasiswa berhasil dihapus!', 'success');
  };

  const cancelDelete = () => {
    showConfirmDelete = false;
    mahasiswaToDelete = null;
  };
</script>

<!-- modal untuk detail -->
<Modal openModal={showDetailModal} on:close={closeDetailModal}>
  {#if selectedMahasiswa}
    <div class="detail-card">
      <div class="avatar">
        <span class="iconify" data-icon="mdi:account-circle" style="font-size: 3.5rem; color: #2563eb;"></span>
      </div>
      <h3 class="detail-nama">{selectedMahasiswa.nama}</h3>
      <div class="detail-info">
        <div class="info-row">
          <span class="iconify info-icon" data-icon="mdi:identifier"></span>
          <span class="info-label">NIM</span>
          <span class="info-value">{selectedMahasiswa.nim}</span>
        </div>
        <div class="info-row">
          <span class="iconify info-icon" data-icon="mdi:school"></span>
          <span class="info-label">Jurusan</span>
          <span class="info-value">{selectedMahasiswa.jurusan}</span>
        </div>
        <div class="info-row">
          <span class="iconify info-icon" data-icon="mdi:star-circle"></span>
          <span class="info-label">IPK</span>
          <span class="info-value">{selectedMahasiswa.ipk}</span>
        </div>
        <div class="info-row">
          <span class="iconify info-icon" data-icon="mdi:check-circle" style="color: {selectedMahasiswa.ipk >= 3 ? '#22c55e' : selectedMahasiswa.ipk >= 2.5 ? '#f59e42' : '#ef4444'}"></span>
          <span class="info-label">Status</span>
          <span class="badge-status {selectedMahasiswa.ipk >= 3 ? 'lulus' : selectedMahasiswa.ipk >= 2.5 ? 'remedial' : 'gagal'}">
            {selectedMahasiswa.ipk >= 3 ? 'Lulus' : selectedMahasiswa.ipk >= 2.5 ? 'Remedial' : 'Tidak Lulus'}
          </span>
        </div>
      </div>
    </div>
  {/if}
</Modal>

<!-- modal untuk tambah -->
<Modal openModal={showFormModal} on:close={closeFormModal}>
  <Form
    {selectedMahasiswa}
    {isEditMode}
    on:addMahasiswa={(e) => {
      if (isEditMode) {
        mahasiswa = mahasiswa.map((mhs) =>
          mhs.id === e.detail.id ? e.detail : mhs
        );
        showAlert('Data mahasiswa berhasil diubah!', 'success');
      } else {
        mahasiswa = [...mahasiswa, e.detail];
        showAlert('Data mahasiswa berhasil ditambahkan!', 'success');
      }
      closeFormModal();
    }}
  />
</Modal>

<Modal openModal={showConfirmDelete} on:close={cancelDelete}>
  <h3>Konfirmasi Hapus</h3>
  <p>Apakah Anda yakin ingin menghapus data mahasiswa ini?</p>
  <div style="display: flex; gap: 1rem; justify-content: flex-end;">
    <button on:click={confirmDelete} style="background: red; color: white; border: none; border-radius: 6px; padding: 8px 16px;">Ya, Hapus</button>
    <button on:click={cancelDelete} style="background: #ccc; color: #333; border: none; border-radius: 6px; padding: 8px 16px;">Batal</button>
  </div>
</Modal>

<Alert message={alertMessage} type={alertType} visible={alertVisible} on:close={() => alertVisible = false} />

<main>
  <div class="container">
    <h1 class="main-title">
      <span class="iconify" data-icon="mdi:account-group" style="vertical-align: middle; margin-right: 0.5rem;"></span>
      Daftar Mahasiswa
    </h1>
    <p>Berikut adalah data mahasiswa beserta status IPK mereka.</p>

    <div class="toolbar">
      <button on:click={openAddForm} class="addBtn">
        <span class="iconify" data-icon="mdi:account-plus" style="vertical-align: middle; margin-right: 0.4rem;"></span>
        Tambah Mahasiswa
      </button>
    </div>

    <div class="table-wrapper">
      <table>
        <thead>
          <tr>
            <th>No</th>
            <th>Nama</th>
            <th>NIM</th>
            <th>Jurusan</th>
            <th>IPK</th>
            <th>Status</th>
            <th>Aksi</th>
          </tr>
        </thead>
        <tbody>
          {#each mahasiswa as mhs, i (mhs.id)}
            <tr>
              <td>{i + 1}</td>
              <td>{mhs.nama}</td>
              <td>{mhs.nim}</td>
              <td>{mhs.jurusan}</td>
              <td
                class:ipk-lulus={mhs.ipk >= 3}
                class:ipk-remedial={mhs.ipk >= 2.5 && mhs.ipk < 3}
                class:ipk-gagal={mhs.ipk < 2.5}
              >
                {mhs.ipk}
              </td>
              <td
                class:ipk-lulus={mhs.ipk >= 3}
                class:ipk-remedial={mhs.ipk >= 2.5 && mhs.ipk < 3}
                class:ipk-gagal={mhs.ipk < 2.5}
              >
                {mhs.ipk >= 3
                  ? "Lulus"
                  : mhs.ipk >= 2.5
                    ? "Remedial"
                    : "Tidak Lulus"}
              </td>
              <td class="aksi-cell">
                <button class="iconBtn editBtn" on:click={() => openEditForm(mhs)} title="Ubah">
                  <span class="iconify" data-icon="mdi:pencil" style="font-size: 1.2rem;"></span>
                </button>
                <button on:click={() => showDetail(mhs)} class="iconBtn detailBtn" title="Detail">
                  <span class="iconify" data-icon="mdi:eye" style="font-size: 1.2rem;"></span>
                </button>
                <button class="iconBtn deleteBtn" on:click={() => handleDelete(mhs.id)} title="Hapus">
                  <span class="iconify" data-icon="mdi:delete" style="font-size: 1.2rem;"></span>
                </button>
              </td>
            </tr>
          {:else}
            <tr>
              <td colspan="7">Tidak ada data mahasiswa</td>
            </tr>
          {/each}
        </tbody>
      </table>
    </div>
  </div>
</main>

<style>
  main {
    padding: 2rem;
    background-color: #f9f9f9;
    min-height: 100vh;
  }
  .container {
    max-width: 960px;
    margin: 0 auto;
    background: white;
    padding: 2rem;
    border-radius: 18px;
    box-shadow: 0 4px 24px rgba(30, 41, 59, 0.10);
  }
  .main-title {
    display: flex;
    align-items: center;
    font-size: 1.7rem;
    font-weight: 700;
    color: #2563eb;
    margin-bottom: 0.25rem;
    text-align: left;
  }
  p {
    margin-bottom: 1rem;
    font-size: 1rem;
    color: #555;
  }
  .toolbar {
    display: flex;
    justify-content: flex-start;
    margin-bottom: 1rem;
  }
  .addBtn {
    background: linear-gradient(90deg, #2563eb 60%, #6366f1 100%);
    color: #fff;
    padding: 10px 18px;
    border: none;
    border-radius: 8px;
    font-weight: bold;
    cursor: pointer;
    font-size: 1.05rem;
    display: flex;
    align-items: center;
    box-shadow: 0 2px 8px 0 #2563eb11;
    transition: background 0.2s, box-shadow 0.2s;
  }
  .addBtn:hover {
    background: linear-gradient(90deg, #1d4ed8 60%, #6366f1 100%);
  }
  .table-wrapper {
    overflow-x: auto;
    border-radius: 14px;
    box-shadow: 0 2px 12px 0 #2563eb0a;
    background: #f8fafc;
    margin-top: 0.5rem;
  }
  table {
    width: 100%;
    border-collapse: separate;
    border-spacing: 0;
    text-align: center;
    background: #fff;
    border-radius: 14px;
    overflow: hidden;
    box-shadow: 0 1px 4px 0 #2563eb0a;
  }
  thead {
    background: linear-gradient(90deg, #2563eb 60%, #6366f1 100%);
    color: white;
  }
  th,
  td {
    padding: 13px 10px;
    font-size: 0.97rem;
    text-transform: uppercase;
    border-bottom: 1.5px solid #f1f5f9;
  }
  th:first-child, td:first-child {
    border-top-left-radius: 14px;
  }
  th:last-child, td:last-child {
    border-top-right-radius: 14px;
  }
  tbody tr {
    transition: background 0.15s;
  }
  tbody tr:hover {
    background: #f1f5fd;
  }
  .ipk-lulus {
    color: #22c55e;
    font-weight: bold;
  }
  .ipk-remedial {
    color: #f59e42;
    font-weight: bold;
  }
  .ipk-gagal {
    color: #ef4444;
    font-weight: bold;
  }
  .aksi-cell {
    display: flex;
    gap: 0.4rem;
    justify-content: center;
  }
  .iconBtn {
    background: #f3f4f6;
    border: none;
    border-radius: 7px;
    padding: 7px 10px;
    cursor: pointer;
    transition: background 0.15s, box-shadow 0.15s;
    display: flex;
    align-items: center;
    box-shadow: 0 1px 4px 0 #2563eb0a;
  }
  .iconBtn:hover {
    background: #e0e7ff;
  }
  .editBtn {
    color: #22c55e;
  }
  .detailBtn {
    color: #2563eb;
  }
  .deleteBtn {
    color: #ef4444;
  }
  /* Detail Modal Styles */
  .detail-card {
    display: flex;
    flex-direction: column;
    align-items: center;
    background: #f8fafc;
    border-radius: 16px;
    box-shadow: 0 2px 12px 0 #2563eb11;
    padding: 1.5rem 1.2rem 1.2rem 1.2rem;
    margin: 0.5rem 0;
    animation: popIn 0.22s cubic-bezier(.4,2,.6,1) both;
  }
  .avatar {
    margin-bottom: 0.5rem;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .detail-nama {
    font-size: 1.25rem;
    font-weight: 700;
    color: #2563eb;
    margin-bottom: 1.1rem;
    margin-top: 0.2rem;
    text-align: center;
  }
  .detail-info {
    width: 100%;
    display: flex;
    flex-direction: column;
    gap: 0.7rem;
  }
  .info-row {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    background: #fff;
    border-radius: 8px;
    padding: 0.5rem 0.8rem;
    box-shadow: 0 1px 4px 0 #2563eb0a;
    font-size: 1.01rem;
    justify-content: flex-start;
  }
  .info-label {
    min-width: 70px;
    color: #64748b;
    font-weight: 500;
    margin-right: 0.5rem;
  }
  .info-value {
    font-weight: 600;
    color: #222;
    margin-left: auto;
  }
  .info-icon {
    font-size: 1.2rem;
    color: #6366f1;
    margin-right: 0.2rem;
  }
  .badge-status {
    display: inline-flex;
    align-items: center;
    font-size: 0.98rem;
    font-weight: 600;
    border-radius: 6px;
    padding: 0.18em 0.7em;
    margin-left: auto;
    box-shadow: 0 1px 4px 0 #2563eb0a;
    letter-spacing: 0.01em;
  }
  .badge-status.lulus {
    background: #e7fbe9;
    color: #22c55e;
  }
  .badge-status.remedial {
    background: #fff7e6;
    color: #f59e42;
  }
  .badge-status.gagal {
    background: #fee2e2;
    color: #ef4444;
  }
  @media (max-width: 640px) {
    .container {
      padding: 1rem;
    }
    table,
    thead,
    tbody,
    th,
    td {
      font-size: 0.85rem;
    }
    .addBtn,
    .iconBtn {
      padding: 6px 10px;
      font-size: 0.85rem;
    }
    .main-title {
      font-size: 1.1rem;
    }
  }
</style>
