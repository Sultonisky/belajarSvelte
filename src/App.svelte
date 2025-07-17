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

  let mahasiswa = [
    {
      id: 1,
      nama: "Moh Sultoni",
      nim: "123456",
      jurusan: "Sistem Informasi",
      ipk: 2.8,
    },
    {
      id: 2,
      nama: "Budi",
      nim: "123457",
      jurusan: "Sistem Informasi",
      ipk: 2.1,
    },
    {
      id: 3,
      nama: "Andi",
      nim: "123458",
      jurusan: "Sistem Informasi",
      ipk: 3.6,
    },
  ];

  const handleDelete = (id) => {
    mahasiswa = mahasiswa.filter((mhs) => mhs.id !== id);
  };
</script>

<main>
  <div class="container">
    <h1>Daftar Mahasiswa</h1>
    <p>Berikut adalah data mahasiswa beserta status IPK mereka.</p>

    <div class="toolbar">
      <button class="addBtn">+ Tambah Mahasiswa</button>
    </div>

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
            <td>
              <button class="editBtn">Ubah</button>
              <button class="deleteBtn" on:click={() => handleDelete(mhs.id)}>
                Hapus
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

  <Modal />
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
    border-radius: 10px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  }

  h1 {
    margin-bottom: 0.25rem;
    font-size: 1.8rem;
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
    background-color: green;
    color: white;
    padding: 10px 16px;
    border: none;
    border-radius: 8px;
    font-weight: bold;
    cursor: pointer;
  }

  table {
    width: 100%;
    border-collapse: collapse;
    text-align: center;
  }

  thead {
    background-color: #ff3e00;
    color: white;
  }

  th,
  td {
    padding: 12px;
    border: 1px solid #ddd;
    font-size: 0.95rem;
    text-transform: uppercase;
  }

  .ipk-lulus {
    color: green;
    font-weight: bold;
  }

  .ipk-remedial {
    color: orange;
    font-weight: bold;
  }

  .ipk-gagal {
    color: red;
    font-weight: bold;
  }

  .editBtn {
    background-color: yellowgreen;
    color: white;
    border: none;
    border-radius: 8px;
    padding: 8px 12px;
    margin-right: 5px;
    cursor: pointer;
  }

  .deleteBtn {
    background-color: red;
    color: white;
    border: none;
    border-radius: 8px;
    padding: 8px 12px;
    cursor: pointer;
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
    .editBtn,
    .deleteBtn {
      padding: 6px 10px;
      font-size: 0.85rem;
    }
  }
</style>
