# REO05-GCC132

* Aluno: Stenio Henrique Machado Silva;
* Curso: Sistemas de Informação; 
* Universidade Federal de Lavras. 

#### *Este é um repositório criado para atender aos requisitos de avaliação do REO05 da disciplina GCC132 - Modelagem e Implementação de Software*

### Descrição 

Foi criado uma página web simples, contendo  uma página web simples, na qual tem uma listagem de alunos com informações referentes ao curso, matrícula, semestre e disciplina. Além disso, é possível cadastrar um novo aluno através do botão Cadastrar aluno que exibe uma modal.  

### Tarefas a serem cumpridas: 
- [X] Criar um README.MD com título, texto formatado, trecho de código, listagem de tópicos, *hyperlinks* e imagens; 
- [ ] O repositório deve conter, pelo menos 3 *commits*.  

### Tecnologias utilizada 

1) Vue JS 5; 
2) HTML. 

### Exemplo de Código utilizado: 
Segue um exemplo de como foi declarado os métodos do Vue: 

```javascript

	<script src="https://cdn.jsdelivr.net/npm/vue@2.x/dist/vue.js"></script>
	<script src="https://cdn.jsdelivr.net/npm/vuetify@2.x/dist/vuetify.js"></script>
	<script src="https://cdnjs.cloudflare.com/ajax/libs/axios/0.20.0/axios.min.js"
		integrity="sha512-quHCp3WbBNkwLfYUMd+KwBAgpVukJu5MncuQaWXgCrfgcxCJAq/fo+oqrRKOj+UKEmyMCG3tb8RB63W+EmrOBg=="
		crossorigin="anonymous"></script>
	<script src="https://cdn.jsdelivr.net/npm/sweetalert2@10.0.2/dist/sweetalert2.all.min.js"></script>

	<script>
		new Vue({
			el: '#app',
			vuetify: new Vuetify(),
			// Componente de retorno de dados 
			data() {
				return {
					alunos: [],
					dialog: false,
					operacao: '',

					aluno: {
						id: '',
						nome: '',
						matricula: '',
						curso: '',
						semestre: '',
						disciplina: '', 
					}
				}
			},

			// Life cycle hook, que irá chamar a função "mostrar" assim que o componente for criado
			created() {
				this.mostrar(); 
			},

			methods: {
				// Função mostrar, responsável por listar todos os alunos cadastrados
				mostrar: function () {
					let opcao = 1; 
					var formData = new FormData(); 
					formData.append("opcao", opcao); 
					// Chamada do Axios, ativando o método post para troca de dados 
					axios.post(url, formData, 
						{ 
							headers: { 
								'Content-Type' : ('multipart/form-data charset=utf-8; boundary=' + Math.random().toString().substr(2))
							}
						})
						.then(response => {
							this.alunos = response.data;
						});
				},
	

				// Função responsável por montar o objeto que será enviado ao backend quando for cadastrado um novo aluno 
				cadastrar: function () {
					let opcao = 2; 
					var formData = new FormData();
					formData.append("nome", this.aluno.nome); 
					formData.append("matricula", this.aluno.matricula); 
					formData.append("curso", this.aluno.curso); 
					formData.append("semestre", this.aluno.semestre); 
					formData.append("disciplina", this.aluno.disciplina);
					formData.append("opcao", opcao); 

					axios.post(url,
						formData, 	
						{
							headers: { 
								'Content-Type' : ('multipart/form-data charset=utf-8; boundary=' + Math.random().toString().substr(2))
							}
						})
						.then(response => {
							this.mostrar();
						});

					this.aluno.nome = "";
					this.aluno.matricula = "";
					this.aluno.curso = "";
					this.aluno.semestre = "";
					this.aluno.disciplina = "";

				},

				// Função que receberá os dados editados e irá montar o objeto para que seja salvo no banco as edições.
				editar: function (id, nome, matricula, curso, semestre, disciplina) {
					let opcao = 3; 
					var formData = new FormData();
					formData.append("id", this.aluno.id);
					formData.append("nome", this.aluno.nome); 
					formData.append("matricula", this.aluno.matricula); 
					formData.append("curso",this.aluno.curso); 
					formData.append("semestre", this.aluno.semestre); 
					formData.append("disciplina", this.aluno.disciplina); 
					formData.append("opcao", opcao);
					
					axios.post(url, 
						formData, 
						{
							headers: { 
								'Content-Type' : ('multipart/form-data charset=utf-8; boundary=' + Math.random().toString().substr(2))
							}	
						})
						.then(response => {
							this.mostrar();
						});
				},

				// Função que deleta o aluno cadastrado
				apagar: function (id) {
					let opcao = 4; 
					var formData = new FormData();
					formData.append("id", id);
					formData.append("opcao", opcao); 

					Swal.fire({
						title: 'Confirma a exclusão do aluno? ',
						confirmButtonText: `Confirmar`,
						showCancelButton: true,
					}).then((result) => {
						if (result.isConfirmed) {
							axios.post(url, formData)
								.then(response => {
									this.mostrar();
								});
							Swal.fire('Apagado!', '', 'success')
						} else if (result.isDenied) {
						}
					});
				},

				// Função responsável por verificar qual operação está sendo realizada no form e chamar a função editar ou cadastrar de acordo com a op realizada
				salvar: function () {
					if (this.operacao == 'cadastrar') {
						this.cadastrar();
					}
					if (this.operacao == 'editar') {
						this.editar();
					}
					this.dialog = false;
				},

				// Abre a modal com um formulário em branco
				novoForm: function () {
					this.dialog = true;
					this.operacao = 'cadastrar';
					this.aluno.nome = '';
					this.aluno.matricula = '';
					this.aluno.curso = '';
					this.aluno.semestre = '';
					this.aluno.disciplina = ''; 

				},

				// Abre a modal com dados pré-preenchidos para edição 
				formEditar: function (id, nome, matricula, curso, semestre, disciplina, cidade, estado, imagem) {
					this.aluno.id = id;
					this.dialog = true;
					this.aluno.nome = nome;
					this.aluno.curso = curso;
					this.aluno.matricula = matricula;
					this.aluno.semestre = semestre;
					this.aluno.disciplina = disciplina; 
					this.dialog = true;
					this.operacao = 'editar';
				},
			}
		});
	</script>

```

### Visual da Página 

![Página inicial](https://user-images.githubusercontent.com/56547429/133850427-4446f734-b255-44b7-a3bc-ef9b442fa5e0.png)

![Cadastrar aluno](https://user-images.githubusercontent.com/56547429/133851565-8a2e1e68-856a-4675-9f81-8b0b7dcbae8c.png)

### Referências

* [Slides disponibilidazos](https://docs.google.com/presentation/d/1Ylr_-kYaAO-g24-viO84H8LpECCr37BArE4vMFT84SQ/edit#slide=id.g6063c73386_0_18); 
* [Vídeo-aula disponibilizada pelo professor](https://www.youtube.com/watch?v=m23SzvELfg4); 
* [Mastering markdown](https://guides.github.com/features/mastering-markdown/).

