# i-need-help
for ask question

## 待解決問題

## 已解決問題

### ansible_sudo_password 抓不到
(感謝 tg ansible taiwan 凍仁翔 Eric chang 指導)
- `ansible_sudo_password` 在 2.9 已經被廢掉了，用 `ansible_become_password`
- 儲存密碼的變數，比如 `a-password` 要改成 `a_password`，python 才讀得到

### 如何將組合好的字串，利用 set_fact 存成一個 List
- 執行指令:ansible-playbook playbook/change_name.yml
- input: [ suisei-v3.22.0.0-PRD-1q2w3e4r.apk, suisei-v3.22.0.0-PRD-1qaz2wsx.apk, suisei-v3.22.0.0-PRD-zaq12wsx.apk ]

- output: [ sui_1q2w3e4r_20210122.apk,sui_1q2w3e4r_20210122.apk,sui_zaq12wsx_20210122.apk ]

但目前執行現有的 playbook，執行 `echo new_apk_name` 這個 task 產生的結果如下
```
TASK [echo new_apk_name] **********************************************************************
ok: [localhost] => (item=results) => {
    "ansible_loop_var": "item",
    "item": "results",
    "sui_zaq12wsx_20210122.apk": "VARIABLE IS NOT DEFINED!"
}
ok: [localhost] => (item=msg) => {
    "ansible_loop_var": "item",
    "item": "msg",
    "sui_zaq12wsx_20210122.apk": "VARIABLE IS NOT DEFINED!"
}
ok: [localhost] => (item=changed) => {
    "ansible_loop_var": "item",
    "item": "changed",
    "sui_zaq12wsx_20210122.apk": "VARIABLE IS NOT DEFINED!"
}

```
解法:(感謝 tg ansible taiwan Yan-ren Tsai  Eric chang 指導)  
(在 commit 4f5f935)  
- set_fact 加附加的項目的方法
```
---
- name: 測試 set_fact 能否附加項目到 List 後面
  hosts: all
  gather_facts: false

  vars:
    foo_list:
      - aaa
      - bbb
      - ccc

  tasks:
    - set_fact:
        foo_list: "{{ foo_list }} + ['ddd']"

    - debug:
        var: foo_list
```
- register 會記錄 set_fact 模組所有的執行結果放到 `new_apk_name_list` 裡