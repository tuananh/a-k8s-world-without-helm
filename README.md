# a-k8s-world-without-helm

> A Kubernetes world without Helm

Từ trước tới giờ mình vẫn prefer Kustomize hơn Helm. Nó đơn giản hơn, easy to grasp và rất dễ để học và extend. Để giải thích Kustomize thì chỉ cần 5 phút là người mới có thể bắt đầu viết được. Còn helm thì ai ko familiar với Go thì hơi ngán 1 chút với Go's template engine.

1 ví dụ với Helm: nếu bạn cần support thêm property nào đó chẳng hạn thì khả năng phải update helm chart là rất cao. Viết 1 chart mà support đầy đủ mọi feature thì khá là mệt. Nhất là khi mọi người đang extend k8s functionalities với CRD và controller/operator rất phổ biến.

Thời điểm cách đây vài năm thì việc support hết các property trong các object phổ biến của k8s thì tuy hơi cực nhưng vẫn là làm được. Thế nhưng controller/operator/CRD bây giờ càng ngày càng nhiều thì việc này là bất khả thi.

> Fun fact: Mình hay nói đùa là để cài Istio, chúng ta cần cài thêm 7x7 49 CustomResourceDefinition (CRD) nhưng con số thực tế là 53 cơ.

1 việc khác là validate và mutate cũng khá khó khăn với các tooling hiện tại nếu ko có sự trợ giúp từ admission controller. Thế nhưng admission controller chỉ có ở server side. 

## Introducing KRM

Trước khi đọc tiếp thì mọi người có thể đọc qua 1 chút về KRM ở đây: https://github.com/kubernetes-sigs/krm-functions-registry

```yaml
pipeline:
  mutators:
    - image: gcr.io/kpt-fn/set-labels:v0.1
      configMap:
        app: wordpress
  validators:
    - image: gcr.io/kpt-fn/kubeval:v0.3
```

Cái này là 1 ví dụ của kpt dùng KRM functions nhưng nó có thể nói lên 1 chút về cách sử dụng của KRM functions.

Nhìn qua thì KRM giống cái gì ?? mấy lệnh của Unix đúng ko? pipe output từ step này sang step tiếp theo.

Thực ra concept này cũng ko mới với kustomize nếu bạn có đọc codebase của kustomize kĩ 1 chút. thế nên nếu ai quen với kustomize thì việc này cũng không quá ngạc nhiên. chẳng qua các step của kustomize ko phải là do user provide ra mà thôi.

KRM cũng mang hơi chút bóng dáng của admission controller, nhưng ở phía client. với KRM, bạn cũng có thể validate hoặc mutate k8s resource object đc như admission controller. the difference is it's a hell lot easier to maintain vs writing rego for example (Looking at you OPA).

Bạn hoàn toàn có thể kết hợp KRM với bất kì orchestrator nào mà bạn đang xài. với unix thì shell như bash là 1 orchestrator chẳng hạn thì với KRM, chúng ta có thể dùng kpt. Ko nhất thiết là kpt mà là có thể bất kì tool nào khác hỗ trợ KRM.

## KRM Function Registry

KRM giới thiệu thêm KRM function registry giúp mọi người có thể sử dụng lại các step phổ biến.

Lợi ích của KRM là việc chúng ta có thể viết các đoạn code ngắn gọn, dễ thay đổi, ko ép buộc phải xài 1 ngôn ngữ cố định nào cả và có thể chain lại thành 1 configuration management pipeline

## kpt.dev
