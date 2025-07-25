# Lucee 6.2.2.66RC: Simple Guide — How I Upgraded to Lucee 6.2.2.66RC from Tomcat 9.0.14 to Tomcat 11.0.9 and JRE 17 (or even better JRE 21) on Ubuntu 20.04 LTS.

This is a simple, step-by-step guide documenting how I manually upgraded an old Lucee installation (originally installed with the Lucee Linux Installer) to **Lucee 6.2.2.66RC** with **Tomcat 11.0.9** and **JRE 17**.

## Step 1: Back Up Everything

Make a **full backup** of the entire server to ensure you can restore everything if needed.

## Step 2: Ensure All Lucee Admin Settings Are Backed Up or Documented

- Whenever possible, configure Lucee settings via `Application.cfc` and **environment variables**—not just in the Server/Web Administrator.
- If you relied on admin-based configurations, consider migrating them. Sometimes upgrades require wiping the `lucee-server` or `web-inf` directories, which store admin settings.
- Moving these settings to code or env variables improves **reliability and portability**.

## Step 3: Follow These Upgrade Steps (as per Git Diff History)

- Download Tomcat 11.0.9 and extract it:  
  `tar -xf apache-tomcat-11.0.9.tar.gz`
- Remove unnecessary Tomcat files: docs, example webapps.
- Add your customized `setenv.sh`.
- Edit `catalina.properties`:  
  - Set `common.loader` to `"${catalina.home}/../lib/*.jar"`
  - Adjust `package.access`, `package.definition`
  - Adapt scanJar-skipper definitions
- Add a `JarScanFilter`
- Modify `server.xml`:  
  - Change HTTP to port 8888, add AJP port connector, etc.
- Edit `web.xml`:  
  - Remove JSP servlet and add the Lucee servlet configuration
- Add the latest Lucee libraries
- Add Adoptium JDK 17

## Step 4: Edit Domain/Host Configs

Update `server.xml` and `tomcat/conf/Catalina/*` for all relevant domain/host configurations.

## Step 5: Set File Permissions

Ensure user/group permissions are set correctly for all new files and directories.

## Step 6: Stop Lucee

Stop the Lucee service before replacing or renaming directories.

## Step 7: Backup Old Directories

Rename current working directories as backups:

- `/tomcat/` → `/tomcat_bak/`
- `/jre/` → `/jre_bak/`
- `/lib/` → `/lib_bak/`

## Step 8: Rename in New Directories

Rename new installations to working directory names:

- `/apache-tomcat-11.0.9_17/` → `/tomcat/`
- `/jre_17/` → `/jre/`
- `/lib_6.2/` → `/lib/`

## Step 9: Restart Lucee

Restart the Lucee service and check Catalina logs for any issues or errors.

## Step 10: Rollback If Needed

If problems arise:

1. Stop Lucee
2. Restore original directory names (undo steps 7 & 8)
3. Restart Lucee

*Always maintain good backups and tailor steps to fit your environment!*
