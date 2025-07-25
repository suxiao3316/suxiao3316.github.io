```bash
cat > oldboyedu-generate-context-conf.sh <<'EOF'
#!/bin/bash
# auther: Jason Yin


# 获取secret的名称
SECRET_NAME=`kubectl get sa linux98 -o jsonpath='{.secrets[0].name}'`

# 指定API SERVER的地址
API_SERVER=10.0.0.231:6443

# 指定kubeconfig配置文件的路径名称
KUBECONFIG_NAME=/tmp/oldboyedu-k8s-dashboard-admin.conf

# 获取用户的tocken
TOCKEN=`kubectl get secrets $SECRET_NAME -o jsonpath={.data.token} | base64 -d`

# 在kubeconfig配置文件中设置群集项
kubectl config set-cluster oldboyedu-k8s-dashboard-cluster --server=$API_SERVER --kubeconfig=$KUBECONFIG_NAME

# 在kubeconfig中设置用户项
kubectl config set-credentials oldboyedu-k8s-dashboard-user --token=$TOCKEN --kubeconfig=$KUBECONFIG_NAME

# 配置上下文，即绑定用户和集群的上下文关系，可以将多个集群和用户进行绑定哟~
kubectl config set-context oldboyedu-admin --cluster=oldboyedu-k8s-dashboard-cluster --user=oldboyedu-k8s-dashboard-user --kubeconfig=$KUBECONFIG_NAME

# 配置当前使用的上下文
kubectl config use-context oldboyedu-admin --kubeconfig=$KUBECONFIG_NAME
EOF

```

