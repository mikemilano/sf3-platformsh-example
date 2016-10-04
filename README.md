# Symfony3, PostgreSQL, Platform.sh Build Problem

## 1) Started with platform.sh 3.0 base on 10/3/2016

The site is preconfigured with mysql and builds fine at this point.


## 2) Added a simple entity `AppBundle\Entity\Foo`

This entity only has an id and name field. Nothing special here.

## 3) Converted to postgresql

- Added the following to `.platform/services.yaml`

```
postgresdb:
    type: postgresql:9.3
    disk: 2048
```

- Added the following to `.platform.app.yaml`
```
runtime:
    extensions:
        - pdo_pgsql

relationships:
    database: "postgresdb:postgresql"
```

At this point, the following code installs and builds schema just fine locally:
```
composer install
bin/console doctrine:database:create
bin/console doctrine:schema:create
```

## 4) Result when pushing code to platform.sh

```
Counting objects: 5, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (5/5), done.
Writing objects: 100% (5/5), 460 bytes | 0 bytes/s, done.
Total 5 (delta 4), reused 0 (delta 0)

Validating submodules.
Validating configuration files.
Processing activity: **Mike Milano** pushed to **Master**
    EnvironmentPushActivity#mv7ekcyxkzldy: Executing activity
    Found 1 new commit.

    Building application 'app' (runtime type: php:7.0, tree: a161a2e)
      Generating runtime configuration.
      
      Moving the application to the output directory
      Prewarming composer cache.
      
      Found a `composer.json`, installing dependencies.
      
      Executing post-build hook...
        
        
                                                                                                               
          [Doctrine\DBAL\Exception\ConnectionException]                                                        
          An exception occured in driver: SQLSTATE[08006] [7] could not connect to server: Connection refused  
          	Is the server running on host "127.0.0.1" and accepting                                             
          	TCP/IP connections on port 5432?                                                                    
                                                                                                               
        
        
        
        
                                                                               
          [Doctrine\DBAL\Driver\PDOException]                                  
          SQLSTATE[08006] [7] could not connect to server: Connection refused  
          	Is the server running on host "127.0.0.1" and accepting             
          	TCP/IP connections on port 5432?                                    
                                                                               
        
        
        
        
                                                                               
          [PDOException]                                                       
          SQLSTATE[08006] [7] could not connect to server: Connection refused  
          	Is the server running on host "127.0.0.1" and accepting             
          	TCP/IP connections on port 5432?                                    
                                                                               
        
        
      
      E: Unknown error building the application: CalledProcessError: Command 'rm web/app_dev.php
      bin/console --env=prod assets:install --no-debug
      ' returned non-zero exit status 1
        File "/usr/lib/python2.7/dist-packages/platformsh_app/builder/__init__.py", line 722, in build
          builder.build()
        File "/usr/lib/python2.7/dist-packages/platformsh_app/builder/__init__.py", line 404, in build
          self.execute_hook()
        File "/usr/lib/python2.7/dist-packages/platformsh_app/builder/__init__.py", line 437, in execute_hook
          env=self.full_env,
        File "/usr/lib/python2.7/dist-packages/platformsh_app/builder/utils.py", line 143, in stream_output
          raise subprocess.CalledProcessError(returncode=p.returncode, cmd=args)
      

    E: Error building the project: Unable to build project, aborting deployment.

    EnvironmentPushActivity#mv7ekcyxkzldy: Activity completed

To myevhanwvk23q@git.us.platform.sh:myevhanwvk23q.git
   cc26b5a..9b879ad  master -> master
```
