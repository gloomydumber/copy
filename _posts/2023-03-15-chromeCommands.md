---
layout: post
title: "ũ�ι̿� ���������� ��ɾ� Ȱ���ϱ�"
date: 2023-03-15 13:20 +0900
categories: WEB-FRONTEND
permalink: /archivers/chromiumCommand
published: true
nocomments: false
use_math: true
---

# Run commands in the Chromium Command Menu

|    ![memoryEater](/assets/posts/2023-03-15-chromeCommands/memoryeatchrome.gif)     |
| :-----------------------------------------------------------------: |
| <b>Run commands in the Chromium Command Menu</b> |

## Quick Refresh

ũ�ι̿� �������� ������ ������ *HTML*, *CSS*, *JavaScript* ������ �̷���� �� ���߿� ���õ� ���� �پ��� ����� �����Ѵ�

�׷��� �پ��� ����� ������ �� ������ ������ UI�� �ƴ� �ܼ� ��ɾ ���� ������ �� �ִµ�, �̸� �˾Ƶθ� ������ ���ϴ� ��ɿ� ������ �� �ִ�.

������ ������ *F12* �� *Ctrl(command) + Shift + I* �� ����Ű�� ������ �� �ְ�, ��ɾ� ����� ������ ������ ���� �� *ctrl(command) + shift + p* �� ���� �����Ѵ�.

�ش� ����Ʈ�� ������ �������� �����ϴ� ��� ������ <b>��ɾ� ���</b>�� ���� �˾ƺ���.

## ������� HTML ��� ������ϱ�

+ `Emulate a focused page` : �Ʒ��� ��ʿ� ���� ������� HTML ��Ҹ� ����� �� �� Ȱ��

![01](/assets/posts/2023-03-15-chromeCommands/first.gif)

�� ���������� ���� ��ʿ� ���� *AutoComplete* �ʵ峪 Ŀ�� ��ġ�� ���� ��Ÿ���ų� ������� HTML ��ҵ��� �ִ�.

�̷��� ��Ҹ� ������ ������ ���� �˻��ϰ��� ��Ŭ������ �˻� ��ư�� Ŭ���ϸ�, Ŭ���� ���� `focus` �� �����Ǿ���� �ش� HTML ��Ҹ� ������� �� ��������.

![02](/assets/posts/2023-03-15-chromeCommands/second.gif)

�� ��, �Ʒ��� ���� ��ɾ� �ֿܼ� `focus` �� �Է����ָ�, `Emulate a focused page` �ɼ��� �����Ѵ�.

![EmulateAfocusedPage](/assets/posts/2023-03-15-chromeCommands/focus.png)

�̷��� �� �Ŀ�, �ٽ� �˻縦 �õ��ϸ� �Ʒ��� ���� ������� �ʰ� ������� ����������.

![03](/assets/posts/2023-03-15-chromeCommands/third.gif)

## Capture

�� �������� �̹����� ĸ�����ִ� ����� �����ϴ� ��ɾ���̴�.

*ctrl(command) + shift + m* ������ *Responsive* ȭ�� ������ �� ���� �����ϸ� ������̳� �º� ȭ�� � ĸ���� �� �ִ�.

+ `Capture area screenshot` : ���콺 Ŀ���� ���ڸ������ �ٲ��, �� ���������� �巡���� �κи� ĸ���Ͽ� �����Ѵ�.
+ `Capture full size screenshot` : ���� ȭ�鿡 ������ �ʴ� *x, y�� ��ũ��* �κ� �� �� ������ UI�� ���θ� ĸ���Ͽ� �����Ѵ�.
+ `Capture node screenshot` : *HTML node Element*�� �����ϰ� �����ϸ�, �ش� ��� �κи��� ĸ���Ͽ� �����Ѵ�.
+ `Capture screenshot` : �����ִ� ȭ�� �״�θ� ĸ���ϸ�, *��ũ�ѹ�* �� �ִ� ���, *��ũ�ѹ�* �� ����� ĸ�ĵȴ�.

## miscellaneous

���� ���ʹ� ��Ÿ ��մ� ��� ���� ����

### Emulate vision deficiencies

+ `Emulate blurred vision` : HTML ��ҵ��� ��� �� ó���Ͽ� ������
+ `Emulate protanopia (no red)` : HTML ��ҿ��� ������ ������ (UI ������ �� ������ ����� �� ���)
+ `Emulate tritanopia (no blue)` : HTML ��ҿ��� û���� ������
+ `Emulate deuteranopia (no green)` : HTML ��ҿ��� ����� ������
+ `Emulate achromatopsia (no color)` : HTML ��ҿ��� ������ �����ϰ� ������� ǥ��

### Developer tool Theme Toggle

+ `Switch to light theme` : ������ ������ �׸��� ����Ʈ �׸��� ����
+ `Switch to dark theme` : ������ ������ �׸��� ��ũ �׸��� ����

### Open File

������ �������� *ctrl(command) + shift + p* �� �ƴ�, *ctrl(command) + p* �� �Է��ϰ� ���������� �����ϴ� ����(*.html*, *.css*, *.js* ���� ����)�� ���ϸ��� �Է��ϸ� �ش� *file* �� �ٷ� �� �� �ִ�.

![openFile](/assets/posts/2023-03-15-chromeCommands/openfile.gif)

## References

[? Chrome Developers ���� �ش��׸�](https://developer.chrome.com/docs/devtools/command-menu/)