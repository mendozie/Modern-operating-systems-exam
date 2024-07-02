# a.	Создание скрипта на языке программирования bash для автоматического архивирования и сжатия данных в операционной системе Linux. Скрипт должен принимать на вход список файлов и папок для архивирования, выбирать метод сжатия (например, gzip или bzip2), создавать архив и вносить информацию о созданном архиве в лог-файл с указанием времени выполнения и объема сжатия. 

## Код:
```bash
#!/bin/bash

# Функция для вывода справки
show_help() {
    echo "Использование: $0 [-c метод_сжатия] [-o имя_архива] файлы..."
    echo
    echo "Опции:"
    echo "  -c   Метод сжатия: gzip или bzip2 (по умолчанию: gzip)"
    echo "  -o   Имя выходного архива (по умолчанию: archive.tar)"
    echo "  файлы...  Список файлов и директорий для архивации"
}

# Переменные по умолчанию
compression="gzip"
output_archive="archive.tar"
log_file="archive_log.txt"

# Обработка аргументов командной строки
while getopts ":c:o:h" opt; do
    case ${opt} in
        c )
            compression=$OPTARG
            ;;
        o )
            output_archive=$OPTARG
            ;;
        h )
            show_help
            exit 0
            ;;
        \? )
            echo "Неверная опция: $OPTARG" 1>&2
            show_help
            exit 1
            ;;
        : )
            echo "Неверная опция: $OPTARG требует аргумент" 1>&2
            show_help
            exit 1
            ;;
    esac
done
shift $((OPTIND -1))

# Проверка наличия файлов и директорий для архивирования
if [ $# -eq 0 ]; then
    echo "Ошибка: не указаны файлы или директории для архивации" 1>&2
    show_help
    exit 1
fi

# Выбор метода сжатия
case $compression in
    gzip)
        tar_command="tar -czf"
        ;;
    bzip2)
        tar_command="tar -cjf"
        ;;
    *)
        echo "Ошибка: неподдерживаемый метод сжатия: $compression" 1>&2
        show_help
        exit 1
        ;;
esac

# Создание архива
$tar_command "$output_archive" "$@"
if [ $? -ne 0 ]; then
    echo "Ошибка: не удалось создать архив" 1>&2
    exit 1
fi

# Получение размера архива
archive_size=$(du -h "$output_archive" | cut -f1)

# Запись информации в лог-файл
echo "[$(date)] Создан архив: $output_archive, Размер: $archive_size, Метод: $compression" >> "$log_file"

# Вывод результата
echo "Архив успешно создан:"
echo "  Архив: $output_archive"
echo "  Размер: $archive_size"
echo "  Метод: $compression"

echo "Запись добавлена в $log_file"

exit 0
```
_____

# Создание скрипта на языке программирования *PowerShell* для автоматического создания архивов в операционной системе Windows с использованием программы *7-Zip*. Скрипт должен автоматически запаковывать файлы и папки с заданным расширением (например, *.txt) из указанной директории, сохранять архивы с уникальными именами и удалить исходные файлы после успешного создания архива.

## Код скрипта:
```powershell
# Запрос пути к директории для архивирования
$sourceDirectory = Read-Host "Введите полный путь к директории для архивирования (например, C:\Path\To\Your\Directory)"

# Проверка, существует ли указанная директория
if (-Not (Test-Path -Path $sourceDirectory)) {
    Write-Output "Указанная директория не существует. Завершение работы скрипта."
    exit
}

# Запрос расширения файлов для архивирования
$fileExtension = Read-Host "Введите тип файлов для архивирования (например, *.txt)"

# Проверка, введено ли корректное расширение
if (-Not ($fileExtension -match "^\*\.\w+$")) {
    Write-Output "Некорректное расширение файлов. Завершение работы скрипта."
    exit
}

# Путь к 7-Zip
$sevenZipPath = "H:\7-Zip\7z.exe"

# Директория для сохранения архива будет той же, что и исходная директория
$destinationDirectory = $sourceDirectory

# Получить текущую дату и время для уникального имени архива
$date = Get-Date -Format "yyyyMMdd_HHmmss"

# Найти файлы с заданным расширением
$files = Get-ChildItem -Path $sourceDirectory -Filter $fileExtension

# Проверить, есть ли файлы для архивации
if ($files.Count -eq 0) {
    Write-Output "Нет файлов с расширением $fileExtension для архивации."
    exit
}

# Уникальное имя архива
$archiveName = "Archive_$date.7z"
$archivePath = Join-Path -Path $destinationDirectory -ChildPath $archiveName

# Создать архив
$filesToArchive = $files | ForEach-Object { $_.FullName }
& $sevenZipPath a $archivePath $filesToArchive -mx=9

# Проверить успешность создания архива
if ($LASTEXITCODE -eq 0) {
    Write-Output "Архив успешно создан: $archivePath"
    
    # Удалить исходные файлы
    $files | Remove-Item -Force
    Write-Output "Исходные файлы удалены."
} else {
    Write-Output "Ошибка при создании архива."
}

# Запись в лог
$logFile = Join-Path -Path $destinationDirectory -ChildPath "archive_log.txt"
$logEntry = "[$(Get-Date)] Создан архив: $archivePath, Файлы: $($filesToArchive -join ', ')"
Add-Content -Path $logFile -Value $logEntry

# Сообщение о завершении
Write-Output "Скрипт успешно завершил работу."
```

