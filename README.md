# k8s-e2e

#进入/kubernetes目录



#进行编译
make -j 8 WHAT=vendor/github.com/onsi/ginkgo/ginkgo
make -j 8 WHAT=test/e2e/e2e.test

（每次修改e2e下面的代码，都要重新make）




#设置环境变量
export KUBE_MASTER_IP=127.0.0.1
export KUBE_MASTER=127.0.0.1
export KUBERNETES_PROVIDER=local

cat > /root/.kube/config << EOF
{
	"User": "root",
	"Password": ""
}
EOF

#并行25个用例，可以节省时间

#Conformance用例并行25个
_output/bin/ginkgo --nodes=25  ./_output/bin/e2e.test -- \
--host="http://127.0.0.1:8080" --provider="local" --ginkgo.v=true --kubeconfig="$HOME/.kube/config" \
--ginkgo.focus="Conformance" --ginkgo.skip="Serial" > /home/lin/e2e/con-0928-1610

#Beethoven用例
_output/bin/ginkgo --nodes=25  ./_output/bin/e2e.test -- \
--host="http://127.0.0.1:8080" --provider="local" --ginkgo.v=true --kubeconfig="$HOME/.kube/config" \
--repo-root="$GOPATH/src/k8s.io/kubernetes" --ginkgo.focus="Beethoven" --ginkgo.skip="Serial"

#跑conformance tests中不能并行跑的tests
_output/bin/e2e.test --host="http://127.0.0.1:8080" --provider="local" --ginkgo.v=true \
--kubeconfig="$HOME/.kube/config" --repo-root="$GOPATH/src/k8s.io/kubernetes" \
--ginkgo.focus="Serial".*"Conformance"

#Beethoven用例串行跑的
_output/bin/e2e.test --host="http://127.0.0.1:8080" --provider="local" --ginkgo.v=true \
--kubeconfig="$HOME/.kube/config" --repo-root="$GOPATH/src/k8s.io/kubernetes" \
--ginkgo.focus="Beethoven.*Serial|Beethoven.*Slow" --ginkgo.skip="node-agent|node_status"

#为了方便查看可以重定向到文件

#跑所有的conformance tests (注意后面两个路径）
_output/bin/e2e.test --host="http://127.0.0.1:8080" --provider="local" --ginkgo.v=true \
--kubeconfig="$HOME/.kube/config" --repo-root="$GOPATH/src/k8s.io/kubernetes" \
--ginkgo.focus="Conformance" > <filename1>

#运行下面命令去掉蓝色文字方便查看
sed -r "s/\x1B\[([0-9]{1,2}(;[0-9]{1,2})?)?[m|K]//g" filename1 > filename2
 
#跑conformance tests中可以并行跑的tests
GINKGO_PARALLEL=y _output/bin/e2e.test --host="http://127.0.0.1:8080" --provider="local" --ginkgo.v=true \
--kubeconfig="$HOME/.kube/config" --repo-root="$GOPATH/src/k8s.io/kubernetes" \
--ginkgo.focus="Conformance" --ginkgo.skip="Serial"
 



#跑(跳过)指定的test，将内容写入--ginkgo.focus(--ginkgo.skip),其为正则匹配。

_output/bin/e2e.test --host="http://127.0.0.1:8080" --provider="local" --ginkgo.v=true \
--kubeconfig="$HOME/.kube/config" --repo-root="$GOPATH/src/k8s.io/kubernetes" \
--ginkgo.focus="should provide Internet connection for containers"

================================
================================
================================
================================
#让程序休眠，time.Minute代表一分钟，10*time.Minute代表十分钟
time.Sleep(10*time.Minute)

_output/bin/e2e.test --host="http://127.0.0.1:8080" --provider="local" --ginkgo.v=true \
--kubeconfig="$HOME/.kube/config" --repo-root="$GOPATH/src/k8s.io/kubernetes" \
--ginkgo.focus="should pull package with valid imagePullSecrets".*"Beethoven"



编译
make -j 8 WHAT=vendor/github.com/onsi/ginkgo/ginkgo 
make -j 8 WHAT=test/e2e/e2e.test


e2e
root@SHA1000161226:/ycj/k8s_V1.5/src/k8s.io/kubernetes/_output/local/bin/linux/amd64# 
GINKGO_PARALLEL=y ./e2e.test --host="http://100.106.227.225:8080" --provider="local" --ginkgo.v=true 
--kubeconfig="$HOME/.kube/config" --repo-root="$GOPATH/src/k8s.io/kubernetes" --ginkgo.focus="Conformance"  
--ginkgo.skip="Serial" >  /tempfile/e2e_result_2017092501.log
