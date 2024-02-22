<script>
    import { onMount } from 'svelte';

    let contatos = [];
    let novoContato = { id: 0, nome: '', telefone: '', endereco: '' };
    let editando = null;

    onMount(() => {
        const contatosSalvos = localStorage.getItem('contatos');
        if (contatosSalvos) {
            contatos = JSON.parse(contatosSalvos);
        }
    });

    function salvar() {
        if (editando !== null) {
            contatos[editando] = { ...novoContato };
            editando = null;
        } else {
            novoContato.id = contatos.length + 1;
            contatos = [...contatos, { ...novoContato }];
        }
        novoContato = { id: 0, nome: '', telefone: '', endereco: '' };
        localStorage.setItem('contatos', JSON.stringify(contatos));
    }

    function editar(index) {
        editando = index;
        novoContato = { ...contatos[index] };
    }

    function deletar(index) {
        contatos = contatos.filter((contato, idx) => idx !== index);
        localStorage.setItem('contatos', JSON.stringify(contatos));
    }
</script>

<h1>CRUD Contatos</h1>

<div>
    <label>
        Nome:
        <input type="text" bind:value={novoContato.nome} />
    </label>
    <label>
        Telefone:
        <input type="text" bind:value={novoContato.telefone} />
    </label>
    <label>
        Endereço:
        <input type="text" bind:value={novoContato.endereco} />
    </label>
    <button on:click={salvar}>Salvar</button>
</div>

<table>
    <thead>
        <tr>
            <th>ID</th>
            <th>Nome</th>
            <th>Telefone</th>
            <th>Endereço</th>
            <th>Ações</th>
        </tr>
    </thead>
    <tbody>
        {#each contatos as contato, index}
            <tr>
                <td>{contato.id}</td>
                <td>{contato.nome}</td>
                <td>{contato.telefone}</td>
                <td>{contato.endereco}</td>
                <td>
                    <button on:click={() => editar(index)}>Editar</button>
                    <button on:click={() => deletar(index)}>Deletar</button>
                </td>
            </tr>
        {/each}
    </tbody>
</table>

<style>
    table {
        border-collapse: collapse;
        width: 100%;
    }
    th, td {
        border: 1px solid #ddd;
        padding: 8px;
        text-align: left;
    }
    th {
        background-color: #f2f2f2;
    }
    button {
        margin-right: 5px;
    }
</style>
