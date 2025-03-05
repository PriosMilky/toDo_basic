<script>
	// Array untuk menyimpan daftar tugas (ToDo)
	let toDoList = [];
	// Variabel untuk menyimpan teks yang dimasukkan oleh pengguna
	let textInput = '';

	// Fungsi untuk menambahkan tugas baru ke dalam toDoList
	function addToDo() {
		toDoList = [...toDoList, { content: textInput, editing: false, checked: false }];
	}

	// Fungsi untuk mengubah status editing (mode edit) dari tugas tertentu
	function setEditing(i, isEditing) {
		toDoList[i].editing = isEditing;
	}

	// Fungsi untuk menghapus tugas dari toDoList berdasarkan indeks
	function deleteTodo(i) {
		toDoList.splice(i, 1);
		toDoList = toDoList;
	}
</script>

<!-- Menambahkan CSS eksternal (Pico CSS) -->
<svelte:head>
	<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@picocss/pico@2/css/pico.min.css" />
</svelte:head>

<!-- Container utama untuk menampilkan ToDo List -->
<div style="margin:0 auto; padding:20px; width: 700px;">
	<h2 style="text-align: center;">ToDO List</h2>
	<p>Enter your ToDo here</p>
	<!-- Input field dan tombol untuk menambahkan tugas -->
	<div style="display: flex;">
		<input type="text" bind:value={textInput} />
		<button style="width: 150px;" on:click={addToDo}>Add</button>
	</div>
</div>

<!-- Loop untuk menampilkan setiap tugas dalam toDoList -->
{#each toDoList as toDo, i}
	<div style="display: flex; align-items: baseline; width: 700px; margin: 0 auto;">
		<!-- Jika tugas sedang dalam mode edit, tampilkan input field untuk mengedit -->
		{#if toDo.editing}
			<input type="text" bind:value={toDo.content} />
			<!-- Jika tidak dalam mode edit, tampilkan checkbox dan teks tugas -->
		{:else}
			<input type="checkbox" bind:checked={toDo.checked} />
			<h4 style="flex-grow: 1;">{toDo.content}</h4>
		{/if}
		<!-- Tombol untuk mengedit/menyimpan dan menghapus tugas -->
		<div style="display: flex;">
			{#if toDo.editing}
				<!-- Tombol "Save" untuk menyimpan perubahan setelah mengedit -->
				<button on:click={() => setEditing(i, false)}>Save</button>
			{:else}
				<!-- Tombol "Edit" untuk mengaktifkan mode edit -->
				<button on:click={() => setEditing(i, true)}>Edit</button>
			{/if}
			<!-- Tombol "Delete" untuk menghapus tugas -->
			<button on:click={() => deleteTodo(i)}>Delete</button>
		</div>
	</div>
{/each}
