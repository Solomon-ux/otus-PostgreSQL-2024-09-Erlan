# Установка и настройка PostgteSQL в контейнере Docker
1. Установил Докер 
![image](https://github.com/user-attachments/assets/13ef1ecb-337d-4e79-b775-976aba89fb66)

2. Заходим на dockerHub и находим официальный образ postgres. 
3. Вводим:
 ```
   $ docker run --name some-postgres -e POSTGRES_PASSWORD=mysecretpassword -d postgres

 ```
![image](https://github.com/user-attachments/assets/a74ef97e-4d12-453d-addc-89f4b653c937)
4. Удаляем контейнер так как необходимо развернуть контейнер с PostgreSQL смонтировав в него /var/lib/postgresql
5. Создаем каталог:
```
mkdir D:\docker\postgres-data
```
6. Создал сеть
```
  docker network create postgres-net
```
7.сервер PostgreSQL с указанием этой сети: 
```
docker run -p 5438:5432 -v D:\docker\postgres-data:/var/lib/postgresql/data --network postgres-net --name post -e POSTGRES_PASSWORD=11111 -d postgres
```
8.Контейнер с клиентом postgres
```
docker run -it --rm --network postgres-net postgres psql -h post -p 5432 -U postgres
```
9. Подключился из контейнера с клиентом к контейнеру с сервером и сделал таблицу с парой строк
![image](https://github.com/user-attachments/assets/88ce0274-ff92-47aa-ab26-362ec251c2b0)

10.подключится к контейнеру с сервером с Dbeaver. Все создано. 
![image](https://github.com/user-attachments/assets/bf9882a1-ea80-454a-b474-d0df1730cf86)

11. Остановил/Удалил контейнер с сервером docker.
```
docker stop post
docker rm post
```    
![image](https://github.com/user-attachments/assets/beb9f29e-25d6-42b0-abf8-6c8dd8977b04)

12.Создаю контейнер с сервером заново
```
docker run -p 5438:5432 -v D:\docker\postgres-data:/var/lib/postgresql/data --network postgres-net --name post -e POSTGRES_PASSWORD=11111 -d postgres
```
13.Подключился снова из контейнера с клиентом к контейнеру с сервером
```
docker run -it --rm --network postgres-net postgres psql -h post -p 5432 -U postgres
```
14.Данные остались на месте
![image](https://github.com/user-attachments/assets/37824cc3-9e3f-4070-8082-9fa373c3699b)

