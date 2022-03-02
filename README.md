## How to install PostgreSQL database for OMOPv5.4 using Docker
1. Goto Athena.ohdsi.org to download Vocabularies.
	./cpt.sh <your_username> <password>
1. Get DDLs from https://github.com/OHDSI/CommonDataModel
2. Get docker image of PostgreSQL: docker pull postgres
3. Run docker run --name omopv54 -d -p 5432:5432 -e POSTGRES_PASSWORD=<password> --restart unless-stopped postgres:latest
4. Create database and schema
create database <databasename> with owner postgres;

5. Run DDL from ODHSI. DDLs are in CommonDataModel folder. Goto postgresql folder.
psql -h localhost -p <port> -U postgres -W -d <database> -f CommonDataModel/OMOP\ CDM\ postgresql\ ddl.txt
5.1 You may want to run the follow if you want to index them.
  psql -h localhost -p <port> -U postgres -W -d <database> -f CommonDataModel-5.4.0/inst/ddl/5.4/postgresql/<ddl_files>
6. Run DDLs for f_observation_view, f_immunizqtion_view, and f_person
  omoponfhir_f_person_ddl.txt
  observation_column_size_change_ddl.txt
  omoponfhir_v5.4_f_observation_view_ddl.txt
  omoponfhir_v5.2_f_immunization_view_ddl.txt
7. Load vocabularies downloaded from Athena.
change the ddl in VocaImport/ to point to the vocabulary files. And change COPY to \copy
9. Use fhir_names ETL to load f_person if your database already has data in it. Read the instruction in fhir_names/ folder.
10. Indexing all IDs in OMOP tables



