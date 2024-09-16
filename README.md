#!/bin/bash

# Verificăm dacă au fost furnizate cel puțin trei argumente
if [ $# -lt 3 ]; then
    echo "Utilizare: $0 <director> <dimensiune_minima> <fisier_iesire>"
    exit 1
fi

# Directorul primit ca argument
my_dir="$1"
# Dimensiunea minimă specificată
min_size="$2"
# Fișierul de ieșire specificat
output_file="$3"

# Funcție recursivă pentru a calcula dimensiunea directorului
calculate_directory_size() {
    local dir="$1"
    local total=0

    # Parcurgem fișierele din director
    for file in "$dir"/*; do
        if [ -f "$file" ]; then
            # Adăugăm dimensiunea fișierului la total
            local file_size=$(stat -c %s "$file")
            total=$((total + file_size))

            # Verificăm dacă dimensiunea fișierului este mai mare decât dimensiunea minimă
            if [ "$file_size" -gt "$min_size" ]; then
                echo "$file_size:$file" >> "$output_file"
            fi
        elif [ -d "$file" ]; then
            # Dacă este un subdirector, apelăm recursiv funcția pentru subdirector
            total=$((total + $(calculate_directory_size "$file")))
        fi
    done

    echo "$total"
}

# Calculăm dimensiunea directorului în bytes
total_size=$(calculate_directory_size "$my_dir")

# Afișăm rezultatul
echo "Total: $total_size bytes"

Total: 8743839 bytes

