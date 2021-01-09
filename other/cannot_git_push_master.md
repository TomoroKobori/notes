## 全てのgitプロジェクトに適用する
- 下記を実行
- 【注意】既存のプロジェクトには反映されない
```
mkdir -p ~/.git_template/hooks
vim ~/.git_template/hooks/pre-push
chmod +x ~/.git_template/hooks/pre-push
git config --global init.templatedir '~/.git_template'
```

- ~/.git_template/hooks/pre-push
```
#!/bin/bash

# pushを禁止するブランチ
readonly MASTER='master'
readonly MAIN='main'
readonly DEVELOP='develop'

while read local_ref local_sha1 remote_ref remote_sha1
do
  if [[ "${remote_ref##refs/heads/}" = $MASTER ]]; then
    echo -e "\033[0;32mDo not push to\033[m\033[1;34m master\033[m \033[0;32mbranch\033[m"
    exit 1
  fi
  if [[ "${remote_ref##refs/heads/}" =~ $MAIN ]]; then
    echo -e "\033[0;32mDo not push to\033[m\033[1;34m main\033[m \033[0;32mbranch\033[m"
    exit 1
  fi
  if [[ "${remote_ref##refs/heads/}" =~ $DEVELOP ]]; then
    echo -e "\033[0;32mDo not push to\033[m\033[1;34m develop\033[m \033[0;32mbranch\033[m"
    exit 1
  fi
done
```

## 個別のプロジェクトで設定を変える
- .git/hooks/pre-push
- 上記に設定ファイルが作成されているので、削除するなり編集するなり