
## Sobre o projeto

Projeto desenvovidocom intuito de fazer listagem. O site foi desenvolvido com ReactJS e React Bootstrap. 

## Pré-requisitos

- [Node.js](https://nodejs.org/en/) (na versão 16 ou superior)
- [NPM](https://www.npmjs.com/)

## Como executar?
```bash
npm install
npm run dev
```

## Passo a passo do desenvolvimento

### 1. Criar o projeto

```bash
npm create vite@latest estudoapp-site --template react
```

### 2. Instalar as dependências

```bash
cd estudoapp-site
npm install react-bootstrap bootstrap
npm install react-router-dom
```

### Importar css do bootstrap

Adicione a seguinte linha em seu `main.jsx`:

```js
import 'bootstrap/dist/css/bootstrap.min.css'
```

Adicione a seguinte linha em seu `index.html`:

```js
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/css/bootstrap.min.css"
    integrity="sha384-rbsA2VBKQhggwzxH7pPCaAqO46MgnOM80zW1RWuH61DGLwZJEdK2Kadq2F9CUG65" crossorigin="anonymous" />
    
```

###  3. Excluir arquivos que não serão utilizados
Exclua os arquivos de configuração inicial do vite, deixando apenas uma div no arquivo App.jsx


### 4. Criando o servidor
Crie a pasta api, e dentro inclua um arquivo com as rotas que foram criadas em sua api. Criando funções para cada ação do back-end.

```js

onst ContentsApi = () => {
  const url = 'http://localhost:3000'

  return {
      getContents () {
          return fetch(`${url}/conteudo`, {
              method: 'GET',
              headers: {
                  'Content-Type': 'application/json'
              }
          })
      },
      createContent (titulo, descricao, porcentagem) {
        return fetch(`${url}/conteudo`, {
          method: 'POST',
          headers: {
              'Content-Type': 'application/json'
          },
          body: JSON.stringify(
            {
              titulo: titulo,
              descricao: descricao,
              porcentagem: porcentagem
            }
          )
       })
      },
  }
}

export default ContentsApi
```

### 5. Cria função para retorno de conteúdo do database
No arquivo App.jsx, crie um useEffect para fazer a chamada para rota get da api e listar o conteúdo salvo.
Antes crie um useState para salvar valores recebidos da api.
Exemplo:

```js
const [contents, setContents] = useState()

  useEffect(() => {
    async function getData() {
      await ContentsApi().getContents().then(data => {
        return data.json()
      })
      .then(data => {
        setContents(data)
      })
    }

    getData()
  }, [])

```
### 6. Listagem dos valores recebidos

Importe da biblioteca `react-bootstrap` os compnentes Table, Container e Button:
```js
import { Table, Container } from 'react-bootstrap'

```
No return do arquivo, primeiro chame o componente `Container` e dentro dele utilize os outros companentes importados:
Exemplo:

```jsx
<Container>
       <Table striped bordered hover>
        <thead>
          <tr>
            <th>Titulo</th>
             <th>Descrição</th>
              <th>Porcentagem</th>
          </tr>
        </thead>

        <tbody>
          {contents && contents.map(cont => (
            <tr key={cont.id}>
              <td>{cont.titulo}</td>
              <td>{cont.descricao}</td>
              <td>{cont.porcentagem}</td>
            </tr>
          ))}
        </tbody>
      </Table>
</Container>
```

###  7. Criando Modal para criação de conteúdo

Crie a pasta components, e um arquivo `createModal.jsx`. Importe os componentes Modal, Button e Form da biblioteca `react-bootstrap`.

```js
import { Modal, Button, Form } from 'react-bootstrap'
```

 No retorno do arquivo, crie uma div para envolver os componentes. Após chame primeiramente o componente `Modal`, dentro dele o `Form` e dentro dele organize os componentes `Modal.Header`, `Modal.Body`, `Modal.Title`, `Form.Group`, `Form.Label`, conforme o layout que você deseja para seu modal.

Exemplo:

```js
<div
    className="modal show"
    style={{ display: 'block', position: 'initial' }}
    >
      <Modal show={props.isModalOpen}>
        <Form onSubmit={(event) => {
          props.createContent(event)
        }}>
        <Modal.Header closeButton onClick={props.handleClose}>
          <Modal.Title>Criar Conteúdo</Modal.Title>
        </Modal.Header>

        <Modal.Body>
          <Form.Group controlId="titulo">
            <Form.Label>
              Titulo
            </Form.Label>
            <Form.Control type="text" />
          </Form.Group>

          <Form.Group controlId="descricao">
            <Form.Label>
              Descrição
            </Form.Label>
            <Form.Control type="text" />
          </Form.Group>

          <Form.Group controlId="porcentagem">
            <Form.Label>
              Porcentagem
            </Form.Label>
            <Form.Control type="number" />
          </Form.Group>
        </Modal.Body>

        <Modal.Footer>
          <Button variant="secondary" onClick={props.handleClose}>Close</Button>
          <Button variant="primary" type="submit">Salvar</Button>
        </Modal.Footer>
        </Form>
      </Modal >
    </div>
```

### 8. Criando Modal para atualização de conteúdo

Dentro da pasta components, crie um novo arquivo `updateModal.jsx`. Importe os componentes Modal, Button e Form da biblioteca `react-bootstrap`. 
```js
import { Modal, Button, Form } from 'react-bootstrap'

```
No retorno do arquivo, crie uma div para envolver os componentes. Após chame primeiramente o componente `Modal`, dentro dele o `Form` e dentro dele organize os componentes `Modal.Header`,`Modal.Body`, `Modal.Title`, `Form.Group`, `Form.Label`, conforme o layout que você deseja para seu modal. Dentro do campo de `Form.Control`, crie a propriedade defaultValue, para receber, via props, os valores atuais do campo que deseja alterar.

Exemplo:
```js
 <div
      className="modal show"
      style={{ display: 'block', position: 'initial' }}
    >
      <Modal show={props.isModalOpen}>
        <Form onSubmit={(event) => {
          props.updateContent(event)
        }}>
        <Modal.Header closeButton onClick={props.handleClose}>
          <Modal.Title>Atualizar Conteúdo</Modal.Title>
        </Modal.Header>

        <Modal.Body>
          <Form.Group controlId="titulo">
            <Form.Label>
              Titulo
            </Form.Label>
            <Form.Control defaultValue={props.content.titulo} type="text" />
          </Form.Group>

          <Form.Group controlId="descricao">
            <Form.Label>
              Descrição
            </Form.Label>
            <Form.Control defaultValue={props.content.descricao} type="text" />
          </Form.Group>

          <Form.Group controlId="porcentagem">
            <Form.Label>
              Porcentagem
            </Form.Label>
            <Form.Control defaultValue={props.content.porcentagem} type="number" />
          </Form.Group>
        </Modal.Body>

        <Modal.Footer>
          <Button variant="secondary" onClick={props.handleClose}>Close</Button>
          <Button variant="primary" type="submit">Salvar</Button>
        </Modal.Footer>
        </Form>
      </Modal >
    </div> 
```
### 9. Incluindo modal de criação no arquivo App.js

Importe os componentes criados.
 ```js
import CreateContentModal from './components/CreateContentModal'
import UpdateContentModal from './components/UpdateContentModal'

 ```

Crie states para controlar a abertura e fechamento de modal de criação, sempre com estado inicial false.

```js
const [isCreateModalOpen, setIsCreateModalOpen] = useState(false)
```

Crie funções para gerir state do modal de creação:
```js
  const handleCloseCreateModal = () => setIsCreateModalOpen(false);
  const handleShowCreateModal = () => setIsCreateModalOpen(true);
```

Crie uma função para chamar endpoint de criação de conteúdo, atualizar conteúdo da tela e alterar estado do modal:

```js
  async function createContent(event) {
    try {
      event.preventDefault()

      const req = event.currentTarget.elements

      await ContentsApi().createContent(
        req.titulo.value, req.descricao.value, Number(req.porcentagem.value)
      ).then(data => {
        return data.json()
      }).then(res => {
        setContents([...contents, {
          id: res.contentId,
          titulo: req.titulo.value,
          descricao: req.descricao.value,
          porcentagem: Number(req.porcentagem.value)
        }])

        setIsCreateModalOpen(false)
      })
    } catch(err) {
      throw err
    }
  }
```

No retorno do arquivo, inclua um fragment para envolver todos os componentes retornados. Depois da abertura do fragmente inclua o componente `Button` para criação de conteúdo. Após o fechamento do componente `Container` importe o componente `CreateModal`. Passe o state que gerencia o fechamento e abertura de modal na propriedade `isModalOpen`, a função de que faz o fechamento no modal na propriedade `handleClose` e a função que faz a criação de conteúdo na propriedade `createContent`:

```js
<>
    <Button
    className="mb-2"
    variant='primary'>
    Criar Conteúdo
    </Button>
    <Conatiner>
        ...
    </Container>
    <CreateModal isModalOpen={isCreateModalOpen} handleClose={handleCloseCreateModal} createContent={createContent} />
<>
```
### 10. Deleção de content

Crie uma função para chamar endpoit de deleção de conteúdo, gerir conteúdos listados:

```js
  async function deleteContent(contentId) {
    try {
      await ContentsApi().deleteContent(contentId)

      const formattedContents = contents.filter(cont => {
        if(cont.id !== contentId){
          return cont
        }
      })

      setContents(formattedContents)
    } catch(err) {
      throw err
    }
  }
  ```


Dentro da tag `tr`, crie um item para ações e inclua o componente `Button` com o evento `onClick` apontando para função deleção criada anteriormente.

```jsx
<Container>
      <Button
        className="mb-2"
        variant='primary'>
        Criar Conteúdo
      </Button>
       <Table striped bordered hover>
        <thead>
          <tr>
            <th>Titulo</th>
             <th>Descrição</th>
              <th>Porcentagem</th>
              <th> 
                     <Button onClick={() => deleteContent(cont.id)} variant='danger'>
                  Excluir
                </Button>
              </th>
          </tr>
        </thead>

        <tbody>
          {contents && contents.map(cont => (
            <tr key={cont.id}>
              <td>{cont.titulo}</td>
              <td>{cont.descricao}</td>
              <td>{cont.porcentagem}</td>
            </tr>
          ))}
        </tbody>
      </Table>
</Container>
```
### 11. Atualização de conteúdo

Importe componente modal de atualização de conteúdo.

```js
import UpdateModal from './components/UpdateContentModal'

```

Crie um state para controlar a abertura e fechamento de modal de atualização, sempre com estado inicial false.

```js
  const [isUpdateModalOpen, setIsUpdateModalOpen] = useState(false)
```

Crie um state para salvar o conteúdo que será atualizado

```js
  const [selectedContent, setSelectedContent] = useState()
```

Crie funções para gerir state do modal de atualização de conteúdo:
```js
  const handleCloseUpdateModal = () => setIsUpdateModalOpen(false);
  const handleShowUpdateModal = () => setIsUpdateModalOpen(true);
```

Crie uma função para chamar endpoint de atualização de conteúdo.

```js

  async function updateContent(event) {
    try {
      event.preventDefault()

      const req = event.currentTarget.elements

      await ContentsApi().updateContent(
        selectedContent.id, req.titulo.value, req.descricao.value, Number(req.porcentagem.value)
      )

      const formattedContents = contents.map(cont => {
        if(cont.id === selectedContent.id) {
          return {
            id: selectedContent.id,
            titulo:  req.titulo.value,
            descricao: req.descricao.value,
            porcentagem: Number(req.porcentagem.value)
          }
        }

        return cont
      })

      setContents(formattedContents)

      setIsUpdateModalOpen(false)
    } catch(err) {
      throw err
    }
  }
```

Dentro da tag de ações, criada no item anterior, inclua mais um componente `Button` com o evento `onClick` apontando para função de atualização de conteúdo. Assim, será possível a abetura do  modal e atualização do conteúdo

```js
    <>
    <Container
      className="
        d-flex
        flex-column
        align-items-start
        justify-content-center
        h-100
        w-100
        "
    >
      <Button
        className="mb-2"
        onClick={handleShowCreateModal}
        variant='primary'>
        Criar Conteúdo
      </Button>
      <Table striped bordered hover>
        <thead>
          <tr>
            <th>Titulo</th>
            <th>Descrição</th>
            <th>Porcentagem</th>
            <th>Ações</th>
          </tr>
        </thead>

        <tbody>
          {contents && contents.map(cont => (
            <tr key={cont.id}>
              <td>{cont.titulo}</td>
              <td>{cont.descricao}</td>
              <td>{cont.porcentagem}</td>
              <td>
                <Button onClick={() => deleteContent(cont.id)} variant='danger'>
                  Excluir
                </Button>
                <Button
                  onClick={() => {
                    handleShowUpdateModal()
                    setSelectedContent(cont)
                  }}
                  variant='warning'
                  className='m-1'
                  >
                  Atualizar
                </Button>
              </td>

            </tr>
          ))}
        </tbody>
      </Table>
    </Container>
    <CreateContentModal isModalOpen={isCreateModalOpen} handleClose={handleCloseCreateModal} createContent={createContent} />
```
Inclua após o componente `Container`, o componente modal para atualização do conteúdo. Passe o state que gerencia o fechamento e abertura de modal na propriedade `isModalOpen`, a função de que faz o fechamento no modal de update na propriedade `handleClose` e o conteúdo a ser atualizado na propriedade `content`:

```js
    <>
    <Container >
      ...
    </Container>
...
    {selectedContent && (
      <UpdateModal isModalOpen={isUpdateModalOpen} handleClose={handleCloseUpdateModal} updateContent={updateContent} content={selectedContent} />
    )}
    </>
```



