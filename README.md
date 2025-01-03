# svntogit

#### 1. svn migration script 다운로드 (atlassian)
    wget https://bitbucket.org/atlassian/svn-migration-scripts/downloads/svn-migration-scripts.jar

#### 2. verify
    dnf install git
    dnf install svn
    dnf install git-svn
    java -jar svn-migration-scripts.jar verify

#### 3. svn 으로 부터 author 정보 얻기
    java -jar svn-migration-scripts.jar authors http://svnsite/svn/project anonsvn anonsvn > authors.txt
authors.txt 편집하기

#### 4. svn → git 이전
    git svn clone --stdlayout --authors-file=authors.txt http://svnsite/svn/project --username anonsvn project

#### 5. 연결 끊기
    cd project
    java -Dfile.encoding=utf-8 -jar ../svn-migration-scripts.jar clean-git --force

#### 6. master push
    git remote add origin https://github.com/gitsite/project.git
    git push origin master

#### 7. branch push
    for branch in `git branch -r`; do git branch $branch refs/remotes/$branch; git push origin $branch; done

#### 8. 100MB 파일 제거 하기 (push 실패시)
    git filter-branch --force --index-filter 'git rm --cached --ignore-unmatch *.VC.db *x86_64.1_78.lib' --prune-empty --tag-name-filter cat -- --all