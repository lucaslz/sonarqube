#!/bin/bash

source color.sh

SONARQUBE_URL=""			# Name of person to greet.
SONARQUBE_LOGIN=""			# Number of greetings to give.
REPOSITORY_PATH=""			# Name of repository

usage() {                               # Function: Print a help message.
   echo -e "\n"
   echo -e "${On_Red}${BIWhite}Usage:${Color_Off} ${On_Green}${BWhite}$0 [ -u SONARQUBE_URL ] [ -l SONARQUBE_LOGIN ] [-r REPOSITORY_PATH] [-h|? HELP]${Color_Off}" 1>&2
   echo -e "\n\n"
}

exit_abnormal() {                        # Function: Exit with error.
  usage
  exit 1
}

sonar_scanner() {
  docker run \
      --rm \
      -e SONAR_HOST_URL="http://${SONARQUBE_URL}" \
      -e SONAR_LOGIN="${SONARQUBE_LOGIN}" \
      -v "${REPOSITORY_PATH}:/usr/src" \
      --network host \
      sonarsource/sonar-scanner-cli:4.7
}

while getopts ":u:l:r:" options; do
  case "${options}" in
    u)
	SONARQUBE_URL="${OPTARG}" ;;
    l)
	SONARQUBE_LOGIN="${OPTARG}" ;;
    r)
        REPOSITORY_PATH="${OPTARG}" ;;
    h|?)
      exit_abnormal ;;
    :)
      echo "Error: -${OPTARG} requires an argument."
      exit_abnormal
     ;;
    *)
      exit_abnormal ;;
  esac
done

if [ -z "${SONARQUBE_URL}" ] || [ -z "${SONARQUBE_LOGIN}" ] || [ -z "${REPOSITORY_PATH}" ]; then
    exit_abnormal
fi

if [ $# -eq 0 ]
  then
    exit_abnormal
fi

sonar_scanner
echo -e "\n\nSONARQUBE_URL: ${SONARQUBE_URL} \nSONARQUBE_LOGIN: ${SONARQUBE_LOGIN} \nREPOSITORY_PATH: ${REPOSITORY_PATH}";
