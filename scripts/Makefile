full_deploy:
	ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/prerequisites.yml
	ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/deploy_cluster.yml
deploy_cluster:
	ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/deploy_cluster.yml
deploy_glusterfs:
	ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/openshift-glusterfs/config.yml
remove_log_met:
	ansible-playbook -e openshift_logging_install_logging=False /usr/share/ansible/openshift-ansible/playbooks/openshift-logging/config.yml
	ansible-playbook -e openshift_metrics_install_metrics=False /usr/share/ansible/openshift-ansible/playbooks/openshift-metrics/config.yml
remove_glusterfs:
	ansible-playbook -e "openshift_storage_glusterfs_wipe=true" /usr/share/ansible/openshift-ansible/playbooks/openshift-glusterfs/uninstall.yml
remove_gluster_with_metrics:
	make remove_log_met
	remove_glusterfs
install_heketi:
	subscription-manager repos --enable=rh-gluster-3-client-for-rhel-7-server-rpms
	yum install heketi-client -y
	cat heketi-var-setup >> ~/.bashrc
	source ~/.bashrc
create_projects:
	oc new-project glusterfs-file
	oc new-project glusterfs-block
create_workload:
	sh pvc-create-and-wait-block.sh
	sh pvc-create-and-wait-file.sh
	sh cirros-create-and-wait-block.sh
	sh cirros-create-and-wait-file.sh
full_deploy_with_workload:
	make full_deploy
	make create_projects
	make create_workload
remove_monitoring:
	ansible-playbook -e openshift_cluster_monitoring_operator_install=false /usr/share/ansible/openshift-ansible/playbooks/openshift-monitoring/config.yml
