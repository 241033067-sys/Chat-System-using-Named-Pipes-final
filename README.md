cd ~
mkdir -p chat_project
cd chat_project
touch chat.txt

cat > chat.sh << 'EOF'

CHAT_FILE="chat.txt"

start_chat() {
  echo "Starting chat... (press Ctrl+C to stop)"
  tail -f "$CHAT_FILE" &
  TAIL_PID=$!

  while true; do
    read msg
    echo "$(date +%H:%M) $msg" >> "$CHAT_FILE"
  done

  kill $TAIL_PID
}

view_chat() {
  echo "----- Chat History -----"
  cat "$CHAT_FILE"
  echo "------------------------"
}

clear_chat() {
  echo -n "" > "$CHAT_FILE"
  echo "Chat cleared!"
}

while true; do
  echo
  echo "========== CHAT MENU =========="
  echo "1. Start Chat"
  echo "2. View Chat History"
  echo "3. Clear Chat"
  echo "4. Exit"
  echo "================================"
  echo -n "Enter your choice: "
  read choice

  case $choice in
    1)
      start_chat
      ;;
    2)
      view_chat
      ;;
    3)
      clear_chat
      ;;
    4)
      echo "Goodbye!"
      exit 0
      ;;
    *)
      echo "Invalid option. Try again."
      ;;
  esac
done
EOF
chmod +x chat.sh
bash chat.sh



<img width="419" height="219" alt="Screenshot 2025-11-19 121247" src="https://github.com/user-attachments/assets/94844a0e-d148-4665-b6a2-9ca89a5f67b8" />



This project is a simple Bash-based chat logger that uses a combination of real-time file monitoring and user input to simulate a live chat environment in a terminal. The setup begins by creating a dedicated directory (`chat_project`) and an empty log file (`chat.txt`). A script named `chat.sh` is then created, and it contains two key components: a background process launched with `tail -f chat.txt &`, which continuously displays new lines appended to the chat log, and an infinite `while true` loop that waits for user input using `read msg`. Every time you type a message and press Enter, the script timestamps it using `date +%H:%M` and appends it to `chat.txt`, causing the background `tail -f` process to immediately show it on the screen, creating the effect of live chat output. Running `bash chat.sh` starts this system, displaying the ongoing chat history while simultaneously allowing new messages to be added. Although it operates entirely on the local machine and supports only one user at a time, it effectively demonstrates core Bash concepts such as background processes, input handling, real-time file observation, and log writing, forming a minimal but functional terminal "chat" interface.
