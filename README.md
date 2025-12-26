# Debian Python alkalmazás futtatása a Docker segítségével 

Hozzunk létre egy könyvtárat a Linux terminál segítségével és egyúttal lépjünk be:

    $ mkdir debian-python-app && cd debian-python-app

A **nano** szerkesztő segítségével írjuk meg a forráskódot Python (.py) fájlkiterjesztéssel:
 
    $ nano app.py

Az **app.py** tartalma:

    def main():
        print("Hello, Docker és Debian!")
        print("A konténer sikeresen fut a Debian alapú környezetben.")

    if __name__ == '__main__':
        main()

Hozzunk létre egy **Docker** fájlt (Dockerfile) a nano segítségével:
 
    $ nano Dockerfile

A **Dockerfile** tartalma:
 
    FROM python:3.11-slim-bookworm

    WORKDIR /app

    COPY app.py /app

    CMD ["python", "app.py"]

**1. A BÁZISKÉP (FROM)**

A **python:3.11-slim-bookworm** a Python hivatalos képének egy minimalista, Debian 12 (Bookworm) alapú verziója. 

Ez szolgáltatja Debian rendszert és a Python futtatókörnyezetet.

**2. A MUNKAKÖNYVTÁR BEÁLLÍTÁSA (WORKDIR)**

Létrehozza és beállítja a konténeren belüli az alapértelmezett könyvtárat.

**3. A KÓD MÁSOLÁSA (COPY)**

A helyi app.py fájlt a konténer /app mappájába másolja.

**4. AZ INDÍTÁSI PARANCS (CMD) MEGHATÁROZÁSA**

Ez a parancs fut le, amikor a konténer elindul.

Listázzuk ki a könyvtár tartalmát:
 
    debian-python-app$ ls
    
A kimenet:
	
	app.py  Dockerfile

Építsük fel a Docker konténert a **build** parancs segítségével:
 
    $ docker build -t debian-python-app .

A folyamat lépései a terminálban:
 
    Sending build context to Docker daemon  3.072kB
    Step 1/4 : FROM python:3.11-slim-bookworm
    3.11-slim-bookworm: Pulling from library/python
    ae4ce04d0e1c: Pull complete                                                     
    5fbbf55f3f6e: Pull complete                                                     
    67d411ce564f: Pull complete                                                     
    88c98e5fb85f: Pull complete                                                     
    Digest: sha256:917ec0e42cd6af87657a768449c2f604a6b67c7ab8e10ff917b8724799f816d3
    Status: Downloaded newer image for python:3.11-slim-bookworm
     ---> cd69d9a7baa1
    Step 2/4 : WORKDIR /app
     ---> Running in 55df138bb552
    Removing intermediate container 55df138bb552
     ---> a9877c69a8cd
    Step 3/4 : COPY app.py /app
     ---> 3a94a49d90b0
    Step 4/4 : CMD ["python", "app.py"]
     ---> Running in 819cfdbfda35
    Removing intermediate container 819cfdbfda35
     ---> 5477a6276798
    Successfully built 5477a6276798
    Successfully tagged debian-python-app:latest
    
Futtassuk a Docker konténert: 

    $ docker run debian-python-app

A kimenet:

	Hello, Docker és Debian!
    A konténer sikeresen fut a Debian alapú környezetben.
 
 A könyvtárra állítsuk be a Git-et: 
 
    $ git init

A kimenet:   
	
	Initialized empty Git repository in /home/bagozoltan/debian-python-app/.git/

Ellenőrizzük a fájlok állapotát (status):

	$ git status

A kimenet:

	On branch main

    No commits yet

    Untracked files:
      (use "git add <file>..." to include in what will be committed)
	    Dockerfile
	    LICENSE.md
	    README.md
	    app.py
	    dockerignore.txt
	    gitignore.txt

    nothing added to commit but untracked files present (use "git add" to track)

Adjuk hozzá az összes (all) fájlt:

    $ git add --all

Ellenőrizzük a fájlok állapotát:

    $ git status

A kimenet:
	
    On branch main

    No commits yet

    Changes to be committed:
      (use "git rm --cached <file>..." to unstage)
  	    new file:   Dockerfile
	    new file:   LICENSE.md
	    new file:   README.md
	    new file:   app.py
	    new file:   dockerignore.txt
	    new file:   gitignore.txt

Hozzuk létre az első commitunkat:

    $ git commit -m "first commit"

A kimenet:

    [main (root-commit) 2a31787] first commit
    6 files changed, 246 insertions(+)
    create mode 100644 Dockerfile
    create mode 100644 LICENSE.md
    create mode 100644 README.md
    create mode 100644 app.py
    create mode 100644 dockerignore.txt
    create mode 100644 gitignore.txt

Nevezzük át az aktuális ágat:

    $ git branch -M main

Állítsuk be a repository elérhetőségét:

    $ git remote add origin git@github.com:ZoltanBago/debian-python-app.git

Töltsük fel a GitHub repository-ba:

    $ git push -u origin main

A kimenet:

    Enter passphrase for key '/home/bagozoltan/.ssh/id_ed25519': 
    Enumerating objects: 9, done.
    Counting objects: 100% (9/9), done.
    Delta compression using up to 2 threads
    Compressing objects: 100% (8/8), done.
    Writing objects: 100% (8/8), 3.74 KiB | 766.00 KiB/s, done.
    Total 8 (delta 0), reused 0 (delta 0), pack-reused 0
    To github.com:ZoltanBago/debian-python-app.git
       4ddfe9c..0975de3  main -> main
    branch 'main' set up to track 'origin/main' by rebasing.

## Licenc

Ez a projekt a [CC BY-NC 4.0](https://creativecommons.org/licenses/by-nc/4.0/) licenc alatt érhető el.  

Szabadon másolható, tanulmányozható és módosítható **nem kereskedelmi célra**, a szerző (Bagó Zoltán) nevének feltüntetése mellett.

Kereskedelmi felhasználás esetén külön engedély szükséges.









