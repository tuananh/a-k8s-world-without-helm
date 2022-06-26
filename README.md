# a-k8s-world-without-helm

> A Kubernetes world without Helm

Từ trước tới h mình vẫn prefer Kustomize hơn Helm. Nó đơn giản hơn, dễ extend hơn.

1 ví dụ với Helm. Nếu bạn cần support thêm property nào đó chẳng hạn thì khả năng phải update helm chart là rất cao. Viết 1 chart mà support đầy đủ mọi feature thì khá là mệt. Nhất là khi mọi người đang extend k8s functionalities với CRD và controller/operator rất phổ biến.

Thời điểm cách đây vài năm thì việc support hết các property trong các object phổ biến của k8s thì tuy hơi cực nhưng vẫn làm được. Thế nhưng controller/operator/CRD bây h càng ngày càng nhiều thì việc này là bất khả thi.

1 việc khác là validate và mutate cũng khá khó khăn với các tooling hiện tại nếu ko có sự trợ giúp từ admission controller. 

## Introducing KRM
nhìn qua thì KRM giống cái gì ?? mấy lệnh của Unix đúng ko? pipe output từ step này sang step tiếp theo.

krm cũng mang hơi chút bóng dáng của admission controller, nhưng ở phía client. với krm, bạn cũng có thể validate hoặc mutate k8s resource object đc như admission controller. the difference is it's a hell lot easier to maintain vs writing rego for example (Looking at you OPA).

bạn hoàn toàn có thể kết hợp krm với bất kì orchestrator nào mà bạn đang xài. với unix thì shell như bash là 1 orchestrator chẳng hạn thì với KRM, chúng ta có thể dùng kpt. Ko nhất thiết là kpt mà là có thể bất kì tool nào khác hỗ trợ KRM.

## KRM Function Registry

krm giới thiệu thêm function registry giúp mọi người có thể sử dụng lại các step phổ biến.

lợi ích của krm là việc chúng ta có thể viết các đoạn code ngắn gọn, dễ thay đổi, ko ép buộc phải xài 1 ngôn ngữ cố định nào cả và có thể chain lại thành 1 configuration management pipeline.

## kpt.dev
