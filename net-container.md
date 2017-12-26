git clone https://github.com/xianlubird/mydocker.git

cd /go/src/github.com/xianlubird/mydocker/

git checkout code-6.5

cd /go/src/github.com/xianlubird/mydocker/network/

go test -run TestAllocate


go test -run TestRelease


