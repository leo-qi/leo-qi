---
layout: post
title: JFileChooser文件选择器几个特殊字符的处理
Date:  2017-2-20
author: "Leo Qi"
catalog: true
tags:
    - Java
    - Swing
    - JFileChooser
---
> 成长没有捷径可走，需要的是一个一个坚实的突破。

## 问题描述 ##

在使用JFileChooser打开文件或文件夹时，采用继承JFileChooser重写approveSelection方法进行文件名称特殊字符过滤时，输入的文件名中包含 \ 或 / 时不能进行保存或打开，会进入上一级或下一级目录，文件名中包含 * 或 ? 时，会重新打开一个文件窗口，并把上次输入的文件名加入到文件类型中，并不能进入上面的approveSelection方法中，也就没有进行校验。

## 解决办法 ##

经调试发现，程序运行到JComponent类中的setUI方法后会出现问题。而JFileChooser又继承自JComponent，所以继续重写JComponent的setUI方法。setUI的参数为ComponentUI类型。在实际运行中传递的是WindowsFileChooserUI子类型。文件名过滤校验放在getApproveSelectionAction中处理。
```
    @Override
    protected void setUI(ComponentUI newUI) {
        super.setUI(new CustomFileChooserUI(this));
    }

    public class CustomFileChooserUI extends ExFileChooserUIWin {

        public CustomFileChooserUI(JFileChooser b) {
            super(b);
        }
        @Override
        public Action getApproveSelectionAction() {
            return new ApproveSelectionAction() {
              @Override
              public void actionPerformed(ActionEvent e) {
                // 文件名过滤处理
                super.actionPerformed(e);
              }
            };
        }
     }
```

## 注意事项 ##

 1. 此解决方案不适合Linux系统。
 2. JDK版本为1.6
 3. 此解决方案可能在Win7以前的系统中可能使文件选择器局部出现透明，在CustomFileChooserUI中接续重写paint方法，可解决。
    ```
    @Override
    public void paint( Graphics g, JComponent c )  {
    	g.setColor(c.getBackground());
    	g.fillRect(0, 0, c.getWidth(), c.getHeight());
    }
    ```
