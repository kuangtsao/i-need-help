# i-need-help
for ask question
## ansible_sudo_password 抓不到
- `ansible_sudo_password` 在 2.9 已經被廢掉了，用 `ansible_become_password`
- 儲存密碼的變數，比如 `a-password` 要改成 `a_password`，python 才讀得到