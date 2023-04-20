#!/bin/bash
print_only=false
super_secure=false

# parse command line options
while getopts ":ps" opt; do
  case $opt in
    p) print_only=true;;
    s) super_secure=true;;
    \?)
      echo "Invalid option: -$OPTARG" >&2
      exit 1;;
  esac
done
shift $((OPTIND -1))

# get the path argument
path="${1%/}"

# loop over all files in the path
shopt -s globstar
for file in "$path"/**/*; do
  if [[ -f "$file" ]]; then

    # get the file extension and name
    filename="${file%.*}"
    extension="${file##*.}"
    if [[ "$extension" == "$filename" ]]; then
      # file has no extension
      extension=""
    fi

    # create the new name
    if [[ -n "$extension" ]]; then
      new_name=$(echo "${filename,,}" | sed 's/ /-/g')
      new_name="${new_name}.${extension}"
    else
      new_name=$(echo "${file,,}" | sed 's/ /-/g')
    fi

    # check if the file needs to be renamed
    if [[ "$new_name" != "$file" ]]; then
      # prompt the user for confirmation if in super-secure mode
      if [ "$super_secure" = true ]; then
        read -p "Rename $file to $new_name? [y/n]: " choice
        case "$choice" in
          y|Y )
            mkdir -p "$(dirname "$new_name")"
            mv -i "$file" "$new_name";;
          n|N ) ;;
          * ) echo "Invalid choice. Skipping $file";;
        esac
      # perform the rename if not in super-secure mode
      else
        if [ "$print_only" = true ]; then
          printf "old: $file\nnew: $new_name\n"
        else
          mkdir -p "$(dirname "$new_name")"
          mv -i "$file" "$new_name"
        fi
      fi
    fi
  fi
done

find "$path" -type d -empty -delete
