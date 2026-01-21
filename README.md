# 네트워크 엔지니어를 위한 Ansible 기반 AWS 보안 그룹(SG) 관리 스크립트
- name: Managed AWS Security Groups for Network Engineers
  hosts: localhost
  connection: local
  gather_facts: False

  vars:
    region: us-west-2
    vpc_id: vpc-0a1b2c3d4e5f6g7h8  # 본인의 VPC ID로 변경하세요

  tasks:
    - name: 웹 서버용 보안 그룹 생성
      amazon.aws.ec2_security_group:
        name: web-server-sg
        description: HTTP 및 SSH 트래픽 허용
        vpc_id: "{{ vpc_id }}"
        region: "{{ region }}"
        rules:
          - proto: tcp
            from_port: 80
            to_port: 80
            cidr_ip: 0.0.0.0/0
            rule_desc: 80번 포트 전체 허용 (HTTP)
          - proto: tcp
            from_port: 22
            to_port: 22
            cidr_ip: 1.2.3.4/32 # 관리자의 실제 IP 주소로 변경하세요
            rule_desc: 특정 관리자 IP에서만 SSH 허용
      register: sg_out

    - name: 결과 출력
      debug:
        msg: "생성된 보안 그룹 ID는 {{ sg_out.group_id }} 입니다."
