FROM ansibleplaybookbundle/apb-base

LABEL "com.redhat.apb.spec"=\
"IyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMj\
IyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMKIyBTaGFyZWQgUGFyYW1ldGVycwojIyMjIyMjIyMjIyMj\
IyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMj\
IyMjIyMjIyMjIwpfcDogJl9wCiAgLSBuYW1lOiBhcGlfa2V5CiAgICB0aXRsZTogQVBJIEtleQog\
ICAgdHlwZTogc3RyaW5nCiAgICBkZXNjcmlwdGlvbjogR2VuZXJhdGVkIGlmIGxlZnQgYmxhbmsK\
ICAgIGRpc3BsYXlfZ3JvdXA6IEFQSSBTZXR0aW5ncwogIC0gbmFtZTogYXBpX3NlY3JldAogICAg\
dGl0bGU6IEFQSSBTZWNyZXQKICAgIHR5cGU6IHN0cmluZwogICAgZGVzY3JpcHRpb246IEdlbmVy\
YXRlZCBpZiBsZWZ0IGJsYW5rCiAgICBkaXNwbGF5X3R5cGU6IHBhc3N3b3JkCiAgICBkaXNwbGF5\
X2dyb3VwOiBBUEkgU2V0dGluZ3MKICAtIG5hbWU6IHBvc3RncmVzcWxfZGF0YWJhc2UKICAgIHRp\
dGxlOiBQb3N0Z3JlU1FMIERhdGFiYXNlIE5hbWUKICAgIHR5cGU6IHN0cmluZwogICAgZGVmYXVs\
dDogZGVtb2RvZ3MKICAgIGRpc3BsYXlfZ3JvdXA6IERhdGFiYXNlIFNldHRpbmdzCiAgICByZXF1\
aXJlZDogVHJ1ZQogIC0gbmFtZTogcG9zdGdyZXNxbF91c2VyCiAgICB0aXRsZTogUG9zdGdyZVNR\
TCBVc2VyCiAgICB0eXBlOiBzdHJpbmcKICAgIGRlZmF1bHQ6IGRlbW91c2VyCiAgICBkaXNwbGF5\
X2dyb3VwOiBEYXRhYmFzZSBTZXR0aW5ncwogICAgcmVxdWlyZWQ6IFRydWUKICAtIG5hbWU6IHBv\
c3RncmVzcWxfcGFzc3dvcmQKICAgIHRpdGxlOiBQb3N0Z3JlU1FMIFBhc3N3b3JkCiAgICB0eXBl\
OiBzdHJpbmcKICAgIGRlZmF1bHQ6IGRlbW9wYXNzCiAgICBkaXNwbGF5X3R5cGU6IHBhc3N3b3Jk\
CiAgICBkaXNwbGF5X2dyb3VwOiBEYXRhYmFzZSBTZXR0aW5ncwogICAgcmVxdWlyZWQ6IFRydWUK\
CgoKIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMj\
IyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMKIyBBUEIgZGVmaW5pdGlvbiAtIGRlbW8tYXBpLWFw\
YgojIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMj\
IyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIwp2ZXJzaW9uOiAxLjAKbmFtZTogZGVtby1hcGktYXBi\
CmRlc2NyaXB0aW9uOiBJbnRlcm5hbCBEb2cgQVBJCmJpbmRhYmxlOiBUcnVlCmFzeW5jOiBvcHRp\
b25hbAptZXRhZGF0YToKICBkaXNwbGF5TmFtZTogSW50ZXJuYWwgRG9nIEFQSSAoQVBCKQogIGlt\
YWdlVXJsOiBodHRwczovL3BuZy5pY29uczguY29tL2RvZy9vZmZpY2UvODAKcGxhbnM6CiAgLSBu\
YW1lOiBkZXYKICAgIGRlc2NyaXB0aW9uOiBUaGUgRG9nIEFQSSBmb3IgZGV2ZWxvcGVycwogICAg\
ZnJlZTogdHJ1ZQogICAgbWV0YWRhdGE6CiAgICAgIGRpc3BsYXlOYW1lOiBEZXZlbG9wbWVudAog\
ICAgICBsb25nRGVzY3JpcHRpb246IFRoaXMgcGxhbiB1c2VzIGEgc2luZ2xlIG5vbi1IQSBQb3N0\
Z3JlU1FMIHNlcnZlciB3aXRob3V0IHBlcnNpc3RlbnQgc3RvcmFnZQogICAgICBjb3N0OiAkMC4w\
MAogICAgcGFyYW1ldGVyczogKl9wCiAgLSBuYW1lOiBwcm9kCiAgICBkZXNjcmlwdGlvbjogVGhl\
IERvZyBBUEkgd2l0aCBwZXJzaXN0ZW50IHN0b3JhZ2UgZm9yIHByb2R1Y3Rpb24gdXNlCiAgICBm\
cmVlOiB0cnVlCiAgICBtZXRhZGF0YToKICAgICAgZGlzcGxheU5hbWU6IFByb2R1Y3Rpb24KICAg\
ICAgbG9uZ0Rlc2NyaXB0aW9uOiBUaGlzIHBsYW4gdXNlcyBhIHNpbmdsZSBub24tSEEgUG9zdGdy\
ZVNRTCBzZXJ2ZXIgd2l0aCBwZXJzaXN0ZW50IHN0b3JhZ2UKICAgICAgY29zdDogJDAuMDAKICAg\
IHBhcmFtZXRlcnM6ICpfcAo="

COPY playbooks /opt/apb/actions
COPY roles /opt/ansible/roles
RUN chmod -R g=u /opt/{ansible,apb}

RUN yum install -y postgresql && yum clean all

USER apb
