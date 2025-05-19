Есть несколько способов связать ваш Bash-скрипт с программой на Go. Вот основные подходы:

## 1. Вызов Bash-скрипта из Go

Вы можете выполнить ваш Bash-скрипт из Go-программы и обработать его вывод:

go

package main

import (
	"fmt"
	"os/exec"
	"strings"
)

func main() {
	// Выполняем Bash-скрипт
	cmd := exec.Command("/bin/bash", "/path/to/your/script.sh")
	output, err := cmd.CombinedOutput()
	if err != nil {
		fmt.Printf("Error executing script: %v\n", err)
		return
	}

	// Обрабатываем вывод
	lines := strings.Split(string(output), "\n")
	for _, line := range lines {
		if strings.Contains(line, "⚠️") {
			// Обработка предупреждений
			fmt.Println("Warning detected:", line)
		}
		// Дополнительная обработка вывода...
	}
}

## 2. Передача данных через аргументы командной строки

Модифицируйте ваш Bash-скрипт для вывода данных в формате, удобном для обработки (например, JSON), а затем прочитайте их в Go:

Bash-скрипт:

bash

#!/bin/bash

# ... ваш существующий код ...

# Вывод в JSON формате
echo "{"
echo "\"disk\": $disk,"
echo "\"disk_message\": \"$disk_message\","
echo "\"cpu\": $cpu,"
echo "\"cpu_message\": \"$cpu_message\","
echo "\"temp\": $temp,"
echo "\"temp_message\": \"$temp_message\","
echo "\"ram_usage\": $ram_usage,"
echo "\"ram_message\": \"$ram_message\","
echo "\"warning\": $warning"
echo "}"

Go-программа:

go

package main

import (
	"encoding/json"
	"fmt"
	"os/exec"
)

type SystemStatus struct {
	Disk        int    `json:"disk"`
	DiskMessage string `json:"disk_message"`
	CPU         int    `json:"cpu"`
	CPUMessage  string `json:"cpu_message"`
	Temp        int    `json:"temp"`
	TempMessage string `json:"temp_message"`
	RAMUsage    int    `json:"ram_usage"`
	RAMMessage  string `json:"ram_message"`
	Warning     bool   `json:"warning"`
}

func main() {
	cmd := exec.Command("/bin/bash", "/path/to/your/script.sh")
	output, err := cmd.Output()
	if err != nil {
		fmt.Printf("Error executing script: %v\n", err)
		return
	}

	var status SystemStatus
	if err := json.Unmarshal(output, &status); err != nil {
		fmt.Printf("Error parsing JSON: %v\n", err)
		return
	}

	fmt.Printf("System status: %+v\n", status)
	if status.Warning {
		fmt.Println("Warning detected!")
		// Дополнительная обработка...
	}
}

## 3. Использование переменных окружения

Вы можете передавать данные через переменные окружения:

Bash-скрипт:

bash

#!/bin/bash

# ... ваш существующий код ...

export DISK_USAGE=$disk
export CPU_USAGE=$cpu
export TEMPERATURE=$temp
export RAM_USAGE=$ram_usage
export WARNING=$warning

# Выполняем Go-программу
/path/to/your/go/program

Go-программа:

go

package main

import (
	"fmt"
	"os"
	"strconv"
)

func main() {
	diskUsage, _ := strconv.Atoi(os.Getenv("DISK_USAGE"))
	cpuUsage, _ := strconv.Atoi(os.Getenv("CPU_USAGE"))
	temp, _ := strconv.Atoi(os.Getenv("TEMPERATURE"))
	ramUsage, _ := strconv.Atoi(os.Getenv("RAM_USAGE"))
	warning := os.Getenv("WARNING") == "true"

	fmt.Printf("Disk: %d%%, CPU: %d%%, Temp: %d°C, RAM: %d%%, Warning: %v\n",
		diskUsage, cpuUsage, temp, ramUsage, warning)
}