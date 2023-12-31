# level5

## How to solve
### Goal1
* <font color="red">***InterfaceA1***</font>に<font color="blue">***InterfaceR1***</font>と同じサブネットマスクを設定する。（省略）
* 既にIPアドレスが設定されている<font color="blue">***InterfaceR1***</font>とネットワーク部は一致し、ホスト部が異なるIPアドレスを<font color="red">***InterfaceA1***</font>に設定する。
* <font color="yellow">***ClientA***</font>のデフォルトルートに<font color="blue">***InterfaceR1***</font>のIPアドレスを設定する。

### Goal2
* <font color="green">***InterfaceB1***</font>に<font color="skayblue">***InterfaceR2***</font>と同じサブネットマスクを設定する。（省略）
* 既にIPアドレスが設定されている<font color="skayblue">***InterfaceR2***</font>とネットワーク部は一致し、ホスト部が異なるIPアドレスを<font color="green">***InterfaceB1***</font>に設定する。
* <font color="cyan">***ClientB***</font>のデフォルトルートに<font color="skayblue">***InterfaceR2***</font>のIPアドレスを設定する。

### Goal3
* <font color="blue">***InterfaceR1***</font>側と<font color="skayblue">***InterfaceR2***</font>側のネットワーク部が別になるように調節する。

## chart
```mermaid
%%{init:{'theme': 'dark'}}%%
flowchart
subgraph LEVEL5
    direction BT
    CA[[ClientA]]
    CB[[ClientB]]
    RR(((RouterR)))
    IA[InterfaceA1]
    IB[InterfaceB1]
    IR1[InterfaceR1]
    IR2[InterfaceR2]
    CA-->IA-->IR1-->RR-->IR2-->IB-->CB
    CB-->IB-->IR2-->RR-->IR1-->IA-->CA
end
```
## example
```mermaid
%%{init:{'theme': 'dark'}}%%
flowchart
Goal3_before-->Goal3_after
subgraph Goal3_before
    direction TB
    G3_BE_RR(((RouterR)))
    G3_BE_CL_A[[ClientA<br>0.0.0.0 => 0.0.0.0]]
    G3_BE_CL_B[[ClientB<br>default => 0.0.0.0]]
    G3_BE_IF_A[0.0.0.0/0.0.0.0]
    G3_BE_IF_B[0.0.0.0/0.0.0.0]
    G3_BE_IF_R1[130.178.246.254/255.255.192.0]
    G3_BE_IF_R2[32.71.7.126/255.255.255.128]
    G3_BE_CL_B<-->G3_BE_IF_B<-->G3_BE_IF_R2<-->G3_BE_RR<-->G3_BE_IF_R1<-->G3_BE_IF_A<-->G3_BE_CL_A
end

subgraph Goal3_after
    direction TB
    G3_AF_RR(((RouterR)))
    G3_AF_CL_A[[ClientA<br>default => 32.71.7.126]]
    G3_AF_CL_B[[ClientB<br>default => 130.178.246.254]]
    G3_AF_IF_A[32.71.7.125/255.255.255.128]
    G3_AF_IF_B[130.178.246.253/255.255.192.0]
    G3_AF_IF_R1[32.71.7.126/255.255.255.128]
    G3_AF_IF_R2[130.178.246.254/255.255.192.0]
    G3_AF_CL_B<-->G3_AF_IF_B<-->G3_AF_IF_R2<-->G3_AF_RR<-->G3_AF_IF_R1<-->G3_AF_IF_A<-->G3_AF_CL_A
end


Goal2_before-->Goal2_after
subgraph Goal2_before
    direction TB
    G2_BE_CL_B[[ClientB<br>default => 0.0.0.0]]
    G2_BE_RR(((RouterR)))
    G2_BE_IF_B[0.0.0.0/0.0.0.0]
    G2_BE_IF_R2[130.178.246.254/255.255.192.0]
    G2_BE_CL_B<-->G2_BE_IF_B<-->G2_BE_IF_R2<-->G2_BE_RR
end

subgraph Goal2_after
    direction TB
    G2_AF_CL_B[[ClientB<br>default => 130.178.246.254]]
    G2_AF_RR(((RouterR)))
    G2_AF_IF_B[130.178.246.253/255.255.192.0]
    G2_AF_IF_R2[130.178.246.254/255.255.192.0]
    G2_AF_CL_B<-->G2_AF_IF_B<-->G2_AF_IF_R2<-->G2_AF_RR
end


Goal1_before-->Goal1_after
subgraph Goal1_before
    direction TB
    G1_BE_CL_A[[ClientA<br>0.0.0.0/0 => 0.0.0.0]]
    G1_BE_RR(((RouterR)))
    G1_BE_IF_A[0.0.0.0/0.0.0.0]
    G1_BE_IF_R1[32.71.7.126/255.255.255.128]
    G1_BE_CL_A<-->G1_BE_IF_A<-->G1_BE_IF_R1<-->G1_BE_RR
end

subgraph Goal1_after
    direction TB
    G1_AF_CL_A[[ClientA<br>default => 32.71.7.126]]
    G1_AF_RR(((RouterR)))
    G1_AF_IF_A[32.71.7.125/255.255.255.128]
    G1_AF_IF_R1[32.71.7.126/255.255.255.128]
    G1_AF_CL_A<-->G1_AF_IF_A<-->G1_AF_IF_R1<-->G1_AF_RR
end
```
