MachacaImagenes
===============
Edición del readme desde Github.<br>
Esto mola XD


finalmente!!!

git filter-branch -f --tree-filter 'cp -f /home/juan/jump_bg_1_stone_statues_new.png /home/juan/MachacaImagenes/imagenes/jump_bg_1_stone_statues.png'



INFORMACIÓN OBTENIDA AQUÍ:

http://qiita.com/wnoguchi/items/62f5e64ef2ae14b4f0ee


gitのコミットの中からパスワードや個人情報等の大事なデータが含まれたものを除去したいときは git filter-branch を使うと便利らしいです。

localhost:example noguchiwataru$ git filter-branch -f --tree-filter 'cp -f /Users/noguchiwataru/Desktop/dummy.png img/confidential.png'
Rewrite 2f712b995fa92c8c1bdadaefd21316ffb2ae9eb7 (9/9)
Ref 'refs/heads/master' was rewritten

オブジェクトがローカルに残っているので gc で抹消します。

localhost:example noguchiwataru$ git gc
Counting objects: 6571, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (4809/4809), done.
Writing objects: 100% (6571/6571), done.
Total 6571 (delta 1662), reused 6544 (delta 1645)

ここでは大事な文字が画像に書き込まれていたというシーンを想定して特定のディレクトリから画像をコピーして上書きしています。削除したいときはrmすればいいと思います。

内部的にはfilterで指定した内容で作業ツリーを書き換えた後にそれぞれのコミットについて再度コミットをかけているようです。

あとはpushするだけですが、再コミットしている段階でリモートブランチと作業ツリーの内容は完全に別物のハッシュになっているので

localhost:example noguchiwataru$ git push
To git@github.com:wnoguchi/example.git
 ! [rejected]        master -> master (non-fast-forward)
error: failed to push some refs to 'git@github.com:wnoguchi/example.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Merge the remote changes (e.g. 'git pull')
hint: before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.

そのままではコンフリクトしてpushできません。なので、-fオプションをつけて強制的にリモートブランチを上書きしています。

localhost:example noguchiwataru$ git push -f
Counting objects: 6531, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (4786/4786), done.
Writing objects: 100% (6531/6531), 52.51 MiB | 1.71 MiB/s, done.
Total 6531 (delta 1628), reused 6531 (delta 1628)
To git@github.com:wnoguchi/example.git
 + 2f712b9...0b6cfa9 master -> master (forced update)

以上で完了。
