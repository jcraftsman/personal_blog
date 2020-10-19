---
title: "Bash Arguments"
date: 2020-03-07T22:52:19+01:00
draft: true
---


```bash
readonly CURRENT_SCRIPT_DIR=$( cd "$(dirname "${BASH_SOURCE[0]}" )" && pwd );

function main
{
  ADDITIONAL_OPTIONS=""
  PARAMS=""
  while (( "$#" )); do
    case "$1" in
      -m|--model)
        FARG=$2
        ADDITIONAL_OPTIONS="${ADDITIONAL_OPTIONS} --model ${2}"
        shift 2
        ;;
      -r|--rand_size)
        ADDITIONAL_OPTIONS="${ADDITIONAL_OPTIONS} --rand_size ${2}"
        shift 2
        ;;
      -d|--hidden_size)
        ADDITIONAL_OPTIONS="${ADDITIONAL_OPTIONS} --hidden_size ${2}"
        shift 2
        ;;
      -s|--share_embedding)
        ADDITIONAL_OPTIONS="${ADDITIONAL_OPTIONS} --share_embedding"
        shift
        ;;
      -s|--use_molatt )
        ADDITIONAL_OPTIONS="${ADDITIONAL_OPTIONS} --use_molatt"
        shift
        ;;
      --) # end argument parsing
        shift
        break
        ;;
      -*|--*=) # unsupported flags
        echo "Error: Unsupported flag $1" >&2
        exit 1
        ;;
      *) # preserve positional arguments
        PARAMS="$PARAMS $1"
        shift
        ;;
    esac
  done
  # set positional arguments in their proper place
  eval set -- "$PARAMS"
    check_arg_exists $1 "You should pass an execution-id as first argument"
  check_arg_exists $2 "You should pass a chemical-property as second argument (choose between: drd2, logp04, logp06, qed)"


  set -o errexit
  set -o pipefail
  set -o nounset
  set -o errtrace
  
}

function usage
{
  echo ""
  echo "usage: iclr19-graph2graph-decode.sh <execution-id> <chemical-property>"
  echo "example: "
  echo "        iclr19-graph2graph-decode.sh f6837fc2-10cf-4485-a1f1-3767c56cebd7 logp06"
  echo ""
}

function check_arg_exists
{
  message=${1}
  arg=${2}
  [ -z "${arg}" ] && echo "${message}" && usage && exit 1;
}

main "$@"
```

- <https://medium.com/@Drew_Stokes/bash-argument-parsing-54f3b81a6a8f>
