# Purpose
Example for liquibase setup to deploy changes to postgres database.

# Project Setup
- Example uses Maven Build tool.
- Liquibase configurations & packaging details are captured in `pom.xml` file.
- Database details for each environment are captured in `pom.xml` file.
- Liquibase changelogs are stored under `src/main/resources` directory.
- To validate liquibase change logs, do `mvn liquibase:validate`
- To test the liquibase changes in database, do:
  ```bash
  mvn liquibase:update
  ```
  By default, profile `local` gets loaded or, you can explicitly mention as: 
  ```bash
  mvn -P local liquibase:update
  ```

# Pipeline
## CI
- Validate liquibase change logs using Maven `liquibase:validate` goal
- Package the pom.xml & changelogs into a zip/tar.gz file using Maven `clean package` goal.
- Create Artifact.

## CD
- Download Artifact.
- Unzip or UnTar the artifact file.
- Deploy liquibase changes using Maven `liquibase:update`. Pass profile according to environment. 
  ```bash
  mvn liquibase:update -P dev -Ddb.password=postgres -Ddb.username=postgres
  ```

# Points to Remember
- In this example, we used "Postgres" driver. If you need liquibase for different flavor of DB, please replace the driver dependency in `pom.xml` file.

# References
- Official Docs on [Liquibase Maven with PostgreSQL](https://docs.liquibase.com/tools-integrations/maven/workflows/creating-liquibase-projects-with-maven-postgresql.html)

