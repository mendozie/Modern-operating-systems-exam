# Управление пользователями и группами. Разграничение доступа. 
> ## P.S. задание не соответствует условию

### a.	В домашнем каталоге (home) создать подкаталоги src, dst и temp;
```
mkdir -p src dst temp
```

### b.	В каталоге temp создать файлы user.txt, root. txt и stud.txt произвольного содержания;
```
echo "This is user file" > /temp/user.txt
echo "This is root file" > /temp/root.txt
echo "This is stud file" > /temp/stud.txt
```

### c.	В каталог src скопировать файлы user.txt, root. txt и stud.txt, различного содержания;
```
cp /temp/user.txt /src/user.txt
cp /temp/root.txt /src/root.txt
cp /temp/stud.txt /src/stud.txt
```

### d.	В каталоге dst создать «жесткие» ссылки на все файлы из каталога src;
```
ln /src/user.txt /dst/user.txt
ln /src/root.txt /dst/root.txt
ln /src/stud.txt /dst/stud.txt
```

### e.	В домашнем каталоге создать «мягкие» ссылки на файлы из каталога src;
```
ln -s /src/user.txt /user_symlink.txt
ln -s /src/root.txt /root_symlink.txt
ln -s /src/stud.txt /stud_symlink.txt
```

### f.	Заархивируйте содержимое папки src/ в архив .tar.gz.
```
tar -czvf src.tar.gz src
```

### g.	Распакуйте этот архив в директорию ~/backup
```
mkdir -p ~/backup
tar -xzvf src.tar.gz -C backup
```

### h.	Выведите названия всех файлов домашней директории, имеющих в названии .txt (подсказка: используйте команду find);
```
find /home -name "*.txt"
```

### i.	Переместите каталог temp в src;
```
mv temp /src/temp
```
