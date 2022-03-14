const abrirModal = () => document.getElementById('modal')
    .classList.add('active')

const fecharModal = () => {
    limparCampos()
    document.getElementById('modal').classList.remove('active')
}


const getLocalStorage = () => JSON.parse(localStorage.getItem('bd_peça')) ?? []
const setLocalStorage = (dbpeça) => localStorage.setItem("bd_peça", JSON.stringify(dbpeça))

// CRUD - create read update delete
const deletarPeça = (index) => {
    const dbpeça = lerPeça()
    dbpeça.splice(index, 1)
    setLocalStorage(dbpeça)
}

const updatePeça = (index, peça) => {
    const dbpeça = lerPeça()
    dbpeça[index] = peça
    setLocalStorage(dbpeça)
}

const lerPeça = () => getLocalStorage('bd_peça')

const criarPeça = (peça) => {
    const dbpeça = getLocalStorage()
    dbpeça.push (peça)
    setLocalStorage(dbpeça)
}

const isValidCampos = () => {
    return document.getElementById('forma').reportValidity()
}

//Interação com o layout

const limparCampos = () => {
    const campos = document.querySelectorAll('.modal-campo')
    campos.forEach(campo => campo.value = "")
    document.getElementById('peça').dataset.index = 'new'
}

const salvarPeça = () => {
    if (isValidCampos()) {
        const peça = {
            peça: document.getElementById('peça').value,
            tipo: document.getElementById('tipo').value,
            preço: document.getElementById('preço').value,
            modelo: document.getElementById('modelo').value
        }
        const index = document.getElementById('peça').dataset.index
        if (index == 'new') {
            criarPeça(peça)
            updatePeça()
            fecharModal()
        } else {
            updatePeça(index, peça)
            updateTabela()
            fecharModal()
        }
    }
}

const criarRow = (peça, index) => {
    const novoRow = document.createElement('tr')
    novoRow.innerHTML = `
        <td>${peça.peça}</td>
        <td>${peça.tipo}</td>
        <td>${peça.preço}</td>
        <td>${peça.modelo}</td>
        <td>
            <button type="button" class="button green" id="edit-${index}">Editar</button>
            <button type="button" class="button red" id="delete-${index}" >Excluir</button>
        </td>
    `
    document.querySelector('#tabelaPeças>tbody').appendChild(novoRow)
}

const limparTabela = () => {
    const rows = document.querySelectorAll('#tabelaPeças>tbody tr')
    rows.forEach(row => row.parentNode.removeChild(row))
}

const updateTabela = () => {
    const dbpeça = lerPeça()
    limparTabela()
    dbpeça.forEach(criarRow)
}

const fillCampos = (peça) => {
    document.getElementById('peça').value = peça.peça
    document.getElementById('tipo').value = peça.tipo
    document.getElementById('preço').value = peça.preço
    document.getElementById('modelo').value = peça.modelo
    document.getElementById('peça').dataset.index = peça.index
}

const editarPeça = (index) => {
    const peça = lerPeça()[index]
    peça.index = index
    fillCampos(peça)
    abrirModal()
}

const editarDelete = (event) => {
    if (event.target.type == 'button') {

        const [action, index] = event.target.id.split('-')

        if (action == 'edit') {
            editarPeça(index)
        } else {
            const peça = lerPeça()[index]
            const response = confirm(`Deseja realmente excluir a categoria ${peça.peça}`)
            if (response) {
                deletarPeça(index)
                updateTabela()
            }
        }
    }
}

updateTabela()

// Eventos
document.getElementById('cadastrarPeças')
    .addEventListener('click', abrirModal)

document.getElementById('modalFechar')
    .addEventListener('click', fecharModal)

document.getElementById('salvar')
    .addEventListener('click', salvarPeça)

document.querySelector('#tabelaPeças>tbody')
    .addEventListener('click', editarDelete)

document.getElementById('cancelar')
    .addEventListener('click', fecharModal)
