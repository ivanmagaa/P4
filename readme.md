# Redes e Contedores en Docker e Docker Compose
 ## Pasos con Docker
 ### 1. Crear unha rede en Docker
 Para crear unha nova rede en Docker, usaremos o seguinte comando:
 ```
 docker network create rede
 ```
 Isto crea unha rede chamada `rede` onde poderemos conectar contedores que queiramos que se comuniquen entre eles.
 ### 2. Crear dous contedores unidos a esa rede
 A continuación, lanzamos dous contedores na rede `rede`:
 ```
 docker run -dit --name contenedor1 --network rede alpine
 docker run -dit --name contenedor2 --network rede alpine
 ```
 Estes comandos crean dous contedores chamados `contenedor1` e `contenedor2`, usando a imaxe `alpine`. 
Ao estar na mesma rede, estes contedores poderán comunicarse entre eles.
 ### 3. Comprobar que os contedores están na rede
 Para verificar os contedores conectados a `rede`:
 ```
 docker network inspect rede
 ```
Este comando amosa os contedores conectados á rede `rede` e información sobre a rede.
 ### 4. Comprobar que os contedores poden verse entre eles
 Probamos a conexión entre contedores usando `ping`:
 ```
 docker exec -it contenedor1 ping contenedor2
 ```
 Se poden verse, `ping` responderá con éxito. Isto indica que a conexión está activa.
 ### 5. Listar as propiedades da rede
 Podemos ver detalles da rede, como os contedores conectados, co comando:
 ```
 docker network inspect rede
 ```
 Esta saída amosará información da rede e enderezos IP asignados.
 ### 6. Crear outra rede e lanzar dous contedores nela
 Creamos unha segunda rede e lanzamos contedores nela:
 ```
 docker network create rede2
 docker run -dit --name contenedor3 --network rede2 alpine
 docker run -dit --name contenedor4 --network rede2 alpine
 ```
 Aquí temos dous contedores (`contenedor3` e `contenedor4`) na nova rede `rede2`.
 ### 7. Comprobar conexións entre os 4 contedores
Para verificar se os contedores poden comunicarse, usamos `ping` entre os contedores.- `contenedor1` e `contenedor2` deben verse entre eles na rede `rede1`.- `contenedor3` e `contenedor4` deben verse entre eles na rede `rede2`.- Non debe haber conexión entre contedores de redes distintas.
 ## Usar Docker Compose
 Docker Compose permite crear un arquivo de configuración para lanzar múltiples contedores e redes.
 ### Pasos Básicos
 1. **Crear un arquivo `docker-compose.yml`**: Aquí definimos os servizos (contenedores) e as redes.
 2. **Definir servizos**: Cada servizo representa un contenedor. Pódese especificar a imaxe, comando, volumes, redes, etc.
 3. **Construír e lanzar**: Usando `docker-compose up` lánzanse os contedores segundo a configuración.
 4. **Escalar servizos**: Con `docker-compose up --scale`, podemos lanzar varias instancias dun servizo.
 ### Exemplo de Configuración para Tres Contedores en Docker Compose
 Creamos un arquivo `docker-compose.yml` co seguinte contido:
 ```yaml
 version: '3'
 services:
  contenedor1:
    image: alpine
    networks:
      - minharede
  contenedor2:
    image: alpine
    networks:
      - minharede
  contenedor3:
    image: alpine
    networks:
      - minharede
 networks:
  minharede:
    driver: bridge
 ```
 Neste arquivo definimos:- `version`: Especifica a versión de Docker Compose.- `services`: Define tres contedores chamados `contenedor1`, `contenedor2` e `contenedor3`.- `image`: Usamos a imaxe `alpine` para os contedores.- `networks`: Ligamos cada contedor á rede `minharede`.- `driver`: Usa `bridge` para permitir a comunicación entre contedores na rede.
 ### Parámetros Adicionais en Docker Compose
 1. **command**: Sobrescribe o comando predeterminado do contedor.
2. **volumes**: Permite compartir datos entre o sistema anfitrión e o contedor.
 3. **environment**: Define variables de entorno para o contedor.
 4. **depends_on**: Define a orde de inicio entre servizos.
