# 1 Setup & Postgres

docker compose up -d
docker ps -a
docker exec -it db psql -U postgres
exit

# 2 Rust with CRUD API 
# Download (win) rust-init.exe - https://www.rust-lang.org/learn/get-started

cargo new backend

Add dependencies in Cargo.toml
Add code to src/main.rs
Add rustapp to compose.yaml 

docker compose build
docker compose up -d rustapp

## [Terminal]: Validate Postgres has table "User"
docker exec -it db psql -U postgres
postgres=# \dt
         List of relations
 Schema | Name  | Type  |  Owner   
--------+-------+-------+----------
 public | users | table | postgres 
(1 row)

# 3 Validate REST API

## Add User: CR
GET     http://localhost:8080/api/rust/users: []
POST    http://localhost:8080/api/rust/users: { "name": "Max", "email": "test" }
GET     http://localhost:8080/api/rust/users: [{ "name": "Max", "email": "test" }]

## Validate in Postgres New Users
docker exec -it db psql -U postgres
postgres=# select * from users;
 id | name | email 
----+------+-------
  1 | Max  | test

## Update user: U
PUT    http://localhost:8080/api/rust/users/1: { "name": "MaxNew", "email": "test" }
GET    http://localhost:8080/api/rust/users: [{ "name": "MaxNew", "email": "test" }]


## Remove user: D
DELETE http://localhost:8080/api/rust/users/1: []
GET    http://localhost:8080/api/rust/users: []

## Validate in Postgres New Users
docker exec -it db psql -U postgres
postgres=# select * from users;
 id | name | email 
----+------+-------
  1 | Max  | test