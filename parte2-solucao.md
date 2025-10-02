### PARTE 02 - RESPOSTAS
----

- Estrutura das tabelas usadas:
  
```sql
-- WARNING: This schema is for context only and is not meant to be run.
-- Table order and constraints may not be valid for execution.

CREATE TABLE public.cargos (
  id_cargos bigint GENERATED ALWAYS AS IDENTITY NOT NULL,
  nome_car_cargo character varying,
  created_at timestamp with time zone NOT NULL DEFAULT now(),
  CONSTRAINT cargos_pkey PRIMARY KEY (id_cargos)
);
CREATE TABLE public.departamento (
  id_dep_depto bigint GENERATED ALWAYS AS IDENTITY NOT NULL,
  nome_dep_depto character varying,
  CONSTRAINT departamento_pkey PRIMARY KEY (id_dep_depto)
);
CREATE TABLE public.funcionarios (
  id_func bigint GENERATED ALWAYS AS IDENTITY NOT NULL,
  nome_func character varying,
  data_contrat date,
  data_demis date,
  salario double precision,
  created_at timestamp with time zone NOT NULL DEFAULT now(),
  id_func_cargo bigint,
  id_func_depto bigint,
  CONSTRAINT funcionarios_pkey PRIMARY KEY (id_func),
  CONSTRAINT funcionarios_id_func_cargo_fkey FOREIGN KEY (id_func_cargo) REFERENCES public.cargos(id_cargos),
  CONSTRAINT funcionarios_id_func_depto_fkey FOREIGN KEY (id_func_depto) REFERENCES public.departamento(id_dep_depto)
);
```

- 2.a:
  
```sql
select 
  f.id_func,
  f.nome_func,
  c.nome_car_cargo,
  d.nome_dep_depto
from funcionarios f
inner join cargos c on c.id_cargos = f.id_func_cargo
inner join departamento d on d.id_dep_depto = f.id_func_depto
order by d.nome_dep_depto, f.nome_func
```


- 2.b:

```sql
select * from funcionarios f
where f.data_demis is null
```


- 2.c:

```sql
select 
  f.id_func,
  f.nome_func,
  d.nome_dep_depto
from funcionarios f
inner join departamento d 
on d.id_dep_depto = f.id_func_depto
and d.nome_dep_depto = 'Controladoria'

--No cenário onde a consulta tivesse como parâmetro código do registro "controladoria". A consulta poderia ser simplificada somente a tabela "funcionarios", exemplo:

select * from funcionarios f
where f.id_func_depto = 2
```


- 2.d:

```sql
select * from funcionarios f
where f.salario > 2900
```


- 2.e:

```sql
select f.id_func_depto, count(*) from funcionarios f
group by f.id_func_depto
```
