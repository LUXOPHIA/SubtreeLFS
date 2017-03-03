# SubtreeLFS
[Subtree](https://git-scm.com/book/ja/v1/Git-%E3%81%AE%E3%81%95%E3%81%BE%E3%81%96%E3%81%BE%E3%81%AA%E3%83%84%E3%83%BC%E3%83%AB-%E3%82%B5%E3%83%96%E3%83%84%E3%83%AA%E3%83%BC%E3%83%9E%E3%83%BC%E3%82%B8) 機能を用いて、[Git-LFS](https://git-lfs.github.com/) の適用された外部リポジトリ「[LibraryLFS](https://github.com/LUXOPHIA/LibraryLFS)」をマージする方法。sss

> **【結論】**マージ先リポジトリにおいて、予め Git-LFS を有効化し、バイナリファイルのコミットを一度でも行えば（その後ハードリセットしても構わない）、Git-LFS の適用された外部リポジトリをサブツリーマージすることができる。  d
> 【Conclusion】In the merge destination repository, if you activate Git-LFS in advance and commit the binary file at least once (You can hard-reset it later), you can subtree-merge external repositories to which Git-LFS applied.

##▼ 失敗１
単純にサブツリーマージしてみる。

1. 「サブツリーの追加/リンク…」を選択。  
[![](https://github.com/LUXOPHIA/SubtreeLFS/raw/README/README/SubtreeLFS-01.png)](https://github.com/LUXOPHIA/SubtreeLFS/raw/README/README/SubtreeLFS-01.png)

1. マージする外部リボジトリの URL と、マージ先のフォルダ名を入力。  
[![](https://github.com/LUXOPHIA/SubtreeLFS/raw/README/README/SubtreeLFS-02.png)](https://github.com/LUXOPHIA/SubtreeLFS/raw/README/README/SubtreeLFS-02.png)

1. エラー  
[![](https://github.com/LUXOPHIA/SubtreeLFS/raw/README/README/SubtreeLFS-03.png)](https://github.com/LUXOPHIA/SubtreeLFS/raw/README/README/SubtreeLFS-03.png)  
>     git -c diff.mnemonicprefix=false -c core.quotepath=false -c credential.helper=manager-st subtree add -P LibraryLFS https://github.com/LUXOPHIA/LibraryLFS master
>     git fetch https://github.com/LUXOPHIA/LibraryLFS master
>     From https://github.com/LUXOPHIA/LibraryLFS
>      * branch            master     -> FETCH_HEAD
>     
>     Downloading LibraryLFS/FLOSVOLARE.png (2.19 MB)
>     Error downloading object: LibraryLFS/FLOSVOLARE.png (5203e7b1f16ac3da8a0657901a097e18581c539aa9366e4c9acb7f2eb24fbf39)
>     
>     Errors logged to X:\SubtreeLFS\.git\lfs\objects\logs\20170304T000343.2862492.log
>     Use `git lfs logs last` to view the log.
>     
>     error: external filter 'git-lfs filter-process' failed
>     fatal: LibraryLFS/FLOSVOLARE.png: smudge filter lfs failed
>     エラー終了しました。エラーの内容は上記をご覧ください。

1. 不正なファイルが追加される。  
[![](https://github.com/LUXOPHIA/SubtreeLFS/raw/README/README/SubtreeLFS-04.png)](https://github.com/LUXOPHIA/SubtreeLFS/raw/README/README/SubtreeLFS-04.png)

##▼ 失敗２
外部リポジトリをサブツリーマージする前に、本リポジトリに Git LFS を適用してみる。

1. 「Track / untrack files…」を選択  
[![](https://github.com/LUXOPHIA/SubtreeLFS/raw/README/README/SubtreeLFS-05.png)](https://github.com/LUXOPHIA/SubtreeLFS/raw/README/README/SubtreeLFS-05.png)

1. 外部リポジトリと同じ拡張子を Git LFS の対象に追加。  
[![](https://github.com/LUXOPHIA/SubtreeLFS/raw/README/README/SubtreeLFS-06.png)](https://github.com/LUXOPHIA/SubtreeLFS/raw/README/README/SubtreeLFS-06.png)

1. 「.gitattributes」ファイルが追加される。  
[![](https://github.com/LUXOPHIA/SubtreeLFS/raw/README/README/SubtreeLFS-07.png)](https://github.com/LUXOPHIA/SubtreeLFS/raw/README/README/SubtreeLFS-07.png)

1. 「.gitattributes」ファイルをコミット。  
[![](https://github.com/LUXOPHIA/SubtreeLFS/raw/README/README/SubtreeLFS-08.png)](https://github.com/LUXOPHIA/SubtreeLFS/raw/README/README/SubtreeLFS-08.png)  
[![](https://github.com/LUXOPHIA/SubtreeLFS/raw/README/README/SubtreeLFS-09.png)](https://github.com/LUXOPHIA/SubtreeLFS/raw/README/README/SubtreeLFS-09.png)

1. 外部リポジトリのサブツリーマージを実行。
[![](https://github.com/LUXOPHIA/SubtreeLFS/raw/README/README/SubtreeLFS-02.png)](https://github.com/LUXOPHIA/SubtreeLFS/raw/README/README/SubtreeLFS-02.png)

1. エラー  
[![](https://github.com/LUXOPHIA/SubtreeLFS/raw/README/README/SubtreeLFS-10.png)](https://github.com/LUXOPHIA/SubtreeLFS/raw/README/README/SubtreeLFS-10.png)  
>     git -c diff.mnemonicprefix=false -c core.quotepath=false -c credential.helper=manager-st subtree add -P LibraryLFS https://github.com/LUXOPHIA/LibraryLFS master
>     git fetch https://github.com/LUXOPHIA/LibraryLFS master
>     From https://github.com/LUXOPHIA/LibraryLFS
>      * branch            master     -> FETCH_HEAD
>     
>     Downloading LibraryLFS/FLOSVOLARE.png (2.19 MB)
>     Error downloading object: LibraryLFS/FLOSVOLARE.png (5203e7b1f16ac3da8a0657901a097e18581c539aa9366e4c9acb7f2eb24fbf39)
>     
>     Errors logged to X:\SubtreeLFS\.git\lfs\objects\logs\20170304T000622.2210813.log
>     Use `git lfs logs last` to view the log.
>     
>     error: external filter 'git-lfs filter-process' failed
>     fatal: LibraryLFS/FLOSVOLARE.png: smudge filter lfs failed
>     エラー終了しました。エラーの内容は上記をご覧ください。

##▼ 成功！

1. 外部リポジトリと同じバイナリファイルを追加。  
[![](https://github.com/LUXOPHIA/SubtreeLFS/raw/README/README/SubtreeLFS-11.png)](https://github.com/LUXOPHIA/SubtreeLFS/raw/README/README/SubtreeLFS-11.png)

1. バイナリファイルをコミット。  
[![](https://github.com/LUXOPHIA/SubtreeLFS/raw/README/README/SubtreeLFS-12.png)](https://github.com/LUXOPHIA/SubtreeLFS/raw/README/README/SubtreeLFS-12.png)  
[![](https://github.com/LUXOPHIA/SubtreeLFS/raw/README/README/SubtreeLFS-13.png)](https://github.com/LUXOPHIA/SubtreeLFS/raw/README/README/SubtreeLFS-13.png)

1. コミットをハードリセット。  
[![](https://github.com/LUXOPHIA/SubtreeLFS/raw/README/README/SubtreeLFS-14.png)](https://github.com/LUXOPHIA/SubtreeLFS/raw/README/README/SubtreeLFS-14.png)  
[![](https://github.com/LUXOPHIA/SubtreeLFS/raw/README/README/SubtreeLFS-15.png)](https://github.com/LUXOPHIA/SubtreeLFS/raw/README/README/SubtreeLFS-15.png)

1. 外部リポジトリのサブツリーマージを実行。
[![](https://github.com/LUXOPHIA/SubtreeLFS/raw/README/README/SubtreeLFS-02.png)](https://github.com/LUXOPHIA/SubtreeLFS/raw/README/README/SubtreeLFS-02.png)

1. 完了  
[![](https://github.com/LUXOPHIA/SubtreeLFS/raw/README/README/SubtreeLFS-16.png)](https://github.com/LUXOPHIA/SubtreeLFS/raw/README/README/SubtreeLFS-16.png)  
>     git -c diff.mnemonicprefix=false -c core.quotepath=false -c credential.helper=manager-st subtree add -P LibraryLFS https://github.com/LUXOPHIA/LibraryLFS master
>     git fetch https://github.com/LUXOPHIA/LibraryLFS master
>     From https://github.com/LUXOPHIA/LibraryLFS
>      * branch            master     -> FETCH_HEAD
>     
>     Added dir 'LibraryLFS'
>     完了しました。

1. 正常にマージされる。
[![](https://github.com/LUXOPHIA/SubtreeLFS/raw/README/README/SubtreeLFS-17.png)](https://github.com/LUXOPHIA/SubtreeLFS/raw/README/README/SubtreeLFS-17.png)
