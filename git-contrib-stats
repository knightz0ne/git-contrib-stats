#!/bin/bash

# Define colors using tput (portable)
GREEN=$(tput setaf 2)
RED=$(tput setaf 1)
NC=$(tput sgr0)

# Header
printf "%-20s %8s %9s %8s\n" "Name" "Added" "Removed" "Net"

git log --all --pretty=format:'%aN' | sort | uniq | while read -r author; do
  stats=$(git log --all --author="$author" --pretty=tformat: --numstat | \
    awk '{ add += $1; subs += $2; } END { printf "%d %d %d\n", add, subs, add - subs }')

  read -r add subs net <<< "$stats"

  # Function to colorize numbers
  colorize() {
    local val=$1
    local padded_val=$2
    if [ "$val" -gt 0 ]; then
      echo "${GREEN}${padded_val}${NC}"
    elif [ "$val" -lt 0 ]; then
      echo "${RED}${padded_val}${NC}"
    else
      echo "${padded_val}"
    fi
  }

  # Pad numbers first (without color)
  add_padded=$(printf "%8d" "$add")
  subs_padded=$(printf "%9d" "$subs")
  net_padded=$(printf "%8d" "$net")

  # Colorize padded numbers
  add_colored=$(colorize "$add" "$add_padded")
  subs_colored=$(colorize "$subs" "$subs_padded")
  net_colored=$(colorize "$net" "$net_padded")

  # Print line with columns aligned and colored numbers
  printf "%-20s %s %s %s\n" "$author" "$add_colored" "$subs_colored" "$net_colored"
done
