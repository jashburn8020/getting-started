# Getting Started with Java and Maven

- Create Maven project using Maven Quickstart archetype in the project (to-be)'s parent directory
  - the `artifactId` value will be used as the project directory name
  - make the `artifactId` value different from the GitHub repository name

```bash
mvn archetype:generate -DarchetypeGroupId=org.apache.maven.archetypes -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.4
```

- Tidy up Maven project
  - delete all unnecessary files in the project directory
  - add an empty `.gitkeep` file to empty directories that you want to keep
    - by default Git excludes empty directories
- Create GitHub repository
  - the repository name should be different from the Maven `artifactId` value
- Clone GitHub repository to the project's parent directory
- Move the contents of the Maven project directory to the cloned repository
  - remove the Maven project directory
  - the GitHub repository is now also the Maven project directory
