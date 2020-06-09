## Laboratory work II

## Tasks

- [x] 1. Создать публичный репозиторий с названием **lab02** и с лиценцией **MIT**
- [x] 2. Сгенирировать токен для доступа к сервису **GitHub** с правами **repo**
- [x] 3. Ознакомиться со ссылками учебного материала
- [x] 4. Выполнить инструкцию учебного материала
- [x] 5. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial


```sh
$ export GITHUB_USERNAME=4Rjee
$ export GITHUB_EMAIL=kochurin.nikita@gmail.com
$ export GITHUB_TOKEN=****************************************
$ alias edit=atom
```

```sh
$ cd ${GITHUB_USERNAME}/workspace
$ source scripts/activate
```

```sh
$ mkdir ~/.config
$ cat > ~/.config/hub <<EOF
github.com:
- user: ${GITHUB_USERNAME}
  oauth_token: ${GITHUB_TOKEN}
  protocol: https
EOF
$ git config --global hub.protocol https
```

```sh
$ mkdir projects/lab02 && cd projects/lab02
# инициализация пустого репозитория в .
$ git init
$ git config --global user.name ${GITHUB_USERNAME}
$ git config --global user.email ${GITHUB_EMAIL}
$ git config -e --global
$ git remote add origin https://github.com/4Rjee/lab02.git
$ git pull origin master
$ touch README.md
$ git status
$ git add README.md
$ git commit -m"added README.md"
# пуш изменений в удаленный репозиторий
$ git push origin master
```

```sh
*build*/
*install*/
*.swp
.idea/
```
В итоге, `git` будет игнорировать файлы, удовлетворяющие _wildcard_ам.

```sh
# выполняем, чтобы получить .gitignore
$ git pull origin master
$ git log
```
Ниже идёт создание различных исходных файлов.

```sh
$ mkdir sources include examples
$ cat > sources/print.cpp <<EOF
#include <print.hpp>
void print(const std::string& text, std::ostream& out)
{
  out << text;
}
void print(const std::string& text, std::ofstream& out)
{
  out << text;
}
EOF
```


```sh
$ cat > include/print.hpp <<EOF

#include <fstream>
#include <iostream>
#include <string>
void print(const std::string& text, std::ofstream& out);
void print(const std::string& text, std::ostream& out = std::cout);

EOF
```

```sh
$ cat > examples/example1.cpp <<EOF
#include <print.hpp>
int main(int argc, char** argv)
{
  print("hello");
}
EOF
```

```sh
$ cat > examples/example2.cpp <<EOF
#include <print.hpp>
#include <fstream>
int main(int argc, char** argv)
{
  std::ofstream file("log.txt");
  print(std::string("hello"), file);
}
EOF
```

```sh
$ edit README.md
```

```sh
# git status покажет, что есть неотслеживаемые изменения
$ git status
# добавим их
$ git add .
# коммит изменений
$ git commit -m"added sources"
# пуш локальных правок в удаленный репозиторий
$ git push origin master
```

## Homework

### Part I

1. Создайте пустой репозиторий на сервисе github.com (или gitlab.com, или bitbucket.com).

2. Выполните инструкцию по созданию первого коммита на странице репозитория, созданного на предыдещем шаге.
    ```bash
    $ echo "# hw2" >> README.md
    $ git init .
    $ git add README.md
    $ git commit -m"added README"
    $ git remote add origin https://github.com/4Rjee/hw2.git
    $ git push -u origin master
    ```
3. Создайте файл `hello_world.cpp` в локальной копии репозитория (который должен был появиться на шаге 2). Реализуйте программу **Hello world** на языке C++ используя плохой стиль кода. Например, после заголовочных файлов вставьте строку `using namespace std;`.
    `hello_world.cpp`:
    ```cpp
    # include <iostream>
    using namespace std;
    int main(int, char**) {
        cout << "Hello, world!" << endl;
        return 0;
    }
    ```
4. Добавьте этот файл в локальную копию репозитория.
    ```bash
    $ git add hello_world.cpp
    ```
5. Закоммитьте изменения с *осмысленным* сообщением.
    ```bash
    $ git commit -m "added hello_world.cpp"
    ```
6. Изменитьте исходный код так, чтобы программа через стандартный поток ввода запрашивалось имя пользователя. А в стандартный поток вывода печаталось сообщение `Hello world from @name`, где `@name` имя пользователя.
    Новый код файла `hello_world.cpp`:
    ```cpp
    # include <iostream>
    using namespace std;
    int main(int, char**) {
        string name;
        getline(cin, name);
        cout << "Hello world from " << name << endl;
        return 0;
    }
    ```
7. Закоммитьте новую версию программы. Почему не надо добавлять файл повторно `git add`?
    `git add` нужен, поскольку файл `hello_world.cpp` не отслеживается
    Однако, альтернативой `git add` в данном случае послужит `git commit -a`.
    ```bash
    $ git add hello_world.cpp
    $ git commit -m "edited hello_world.cpp"
    ```
8. Запуште изменения в удалёный репозиторий.
    ```bash
    $ git push origin master
    ```

### Part II

**Note:** *Работать продолжайте с теми же репоззиториями, что и в первой части задания.*
1. В локальной копии репозитория создайте локальную ветку `patch1`.
    ```bash
    $ git checkout -b patch1
    ```
2. Внесите изменения в ветке `patch1` по исправлению кода и избавления от `using namespace std;`.    
    Новый код файла `hello_world.cpp`:
    ```cpp
    # include <iostream>
    int main(int, char**) {
        std::string name;
        std::getline(std::cin, name);
        std::cout << "Hello world from " << name << std::endl;
        return 0;
    }
    ```
3. **commit**, **push** локальную ветку в удалённый репозиторий.
    ```bash
    $ git commit -am "code style edits"
    $ git push origin patch1
    ```
6. В локальной копии в ветке `patch1` добавьте в исходный код комментарии.
    ```cpp
    # include <iostream>
    int main(int, char**) {
        std::string name;
        // getting user's name from stdin
        std::getline(std::cin, name);
        std::cout << "Hello world from " << name << std::endl;
        return 0;
    }
    ```
7. **commit**, **push**.
    ```bash
    $ git commit -am "code style edits"
    $ git push origin patch1
    ```
10. Локально выполните **pull**.
    ```bash
    # снова отслеживаем master
    $ git checkout master
    $ git pull origin master
    ```
12. Удалите локальную ветку `patch1`.
    ```bash
    $ git branch -d patch1
    ```

### Part III

**Note:** *Работать продолжайте с теми же репоззиториями, что и в первой части задания.*
1. Создайте новую локальную ветку `patch2`.
    ```bash
    $ git checkout -b patch2
    ```
2. Измените *code style* с помощью утилиты [**clang-format**](http://clang.llvm.org/docs/ClangFormat.html). Например, используя опцию `-style=Mozilla`.
    ```bash
    $ sudo apt install clang-format
    $ clang-format -style=Mozilla -i hello_world.cpp
    ```
3. **commit**, **push**, создайте pull-request `patch2 -> master`.
    ```bash
    $ git add .
    $ git commit -m "code style edits"
    $ git push origin patch2
    ```
6. Локально выполните **pull** + **rebase** (точную последовательность команд, следует узнать самостоятельно). **Исправьте конфликты**.
    ```bash
    $ git checkout master
    $ git pull origin master
    # здесь будет ошибка"
    $ git rebase patch2
    ```
7. Сделайте *force push* в ветку `patch2`
    ```bash
    $ git push origin patch2 --force
    ```
