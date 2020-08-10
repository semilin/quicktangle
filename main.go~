package main

import (
	"os"
	"log"
	"bufio"
	"strings"
	"runtime/debug"
)

func handleErr(e error) {
	if e != nil {
		log.Fatal(e)
	}
}

func main() {
	debug.SetGCPercent(0)
	args := os.Args[1:]

	if len(args) < 2 {
		log.Fatal("Quicktangle requires input and output paths, only an input was provided.")
	} else if len(args) > 2 {
		log.Fatal("Quicktangle requires 2 arguments, extras were provided.")
	}

	os.Remove(args[1])
	in, err := os.Open(args[0])
	handleErr(err)

	out, err := os.Create(args[1])
	handleErr(err)

	defer in.Close()
	defer out.Close()

	scanner := bufio.NewScanner(in)
	in_src := false
	for scanner.Scan() {
		line := scanner.Text()
		line = strings.TrimSpace(line)
		if in_src {
			if strings.HasPrefix(strings.ToLower(line), "#+end_src") {
				in_src = false
				continue
			} else {
				if (line == "" || strings.HasPrefix(line, ";")) {
					continue
				} else {
					out.WriteString(line)
					out.WriteString("\n")	
				}
			}
		} else {
			if strings.HasPrefix(strings.ToLower(line), "#+begin_src") {
				in_src = true
				continue
			}
		}
	}

}
