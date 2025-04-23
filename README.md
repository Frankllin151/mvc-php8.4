#Para utilizar o framework:

#Clone o repositório e inicialize o ambiente:
 -docker-compose up -d

#Instale as dependências:
 -composer install


#Crie a estrutura do banco de dados
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    email VARCHAR(255) NOT NULL UNIQUE,
    password VARCHAR(255) NOT NULL,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);



Entendendo a estrutura do projeto:
Diretórios principais:

app/: Contém toda a lógica da aplicação (MVC)
public/: Ponto de entrada da aplicação (único diretório exposto)
config/: Arquivos de configuração
.docker/: Configurações do Docker
storage/: Arquivos gerados pela aplicação (logs, caches)

Fluxo da aplicação:

Uma requisição chega ao public/index.php
O autoloader do Composer e variáveis de ambiente são carregados
A aplicação é inicializada via App.php
O Router captura a URL e método da requisição
O Router identifica qual controller/método deve ser chamado
Os middlewares aplicáveis são executados
O controller processa a requisição e retorna uma resposta

Como adicionar novos recursos:
Criar um novo modelo:

Crie uma classe em app/Models/ que estenda BaseModel
Defina a tabela e campos permitidos:
phpprotected string $table = 'products';
protected array $fillable = ['name', 'price', 'description'];


Criar um novo controller:

Crie uma classe em app/Controllers/ que estenda BaseController
Implemente os métodos necessários (index, show, store, update, destroy)

Adicionar novas rotas:

Edite app/Routes/web.php para rotas da web ou app/Routes/api.php para endpoints da API
Use o router para definir suas rotas:
php$router->get('/products', [ProductController::class, 'index']);
$router->post('/products', [ProductController::class, 'store']);


Criar rotas protegidas:
Use o método middleware antes de definir a rota:
php$router->middleware('AuthMiddleware')->get('/admin/users', [AdminController::class, 'index']);
Criar um novo middleware:

Crie uma classe em app/Middleware/ que implemente MiddlewareInterface
Implemente o método handle() com sua lógica de verificação