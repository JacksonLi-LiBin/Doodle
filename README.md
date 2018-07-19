# Doodle

Image doodle for Android. You can undo, zoom, move, add text, textures, etc. Also, a powerful, customizable and extensible doodle framework.

适用于Android的图像涂鸦，具有撤消、缩放、移动、添加文字，贴图等功能。还是一个功能强大，可自定义和可扩展的涂鸦框架。

![]()

# Feature

  * Brushes and shapes

    The brush can choose hand-painted, imitation, eraser, text, texture, and the imitation function is similar to that in PS, copying somewhere in the picture. Shapes can be selected from hand-drawn, arrows, lines, circles, rectangles, and so on. The background color of the brush can be selected as a color, or an image.

  * Undo

    Each step of the doodle operation can be undone.

  * Zoom, move, and rotate

    In the process of doodle, you can freely zoom and move the picture with gestures. Support for rotating pictures.

  * Amplifier

    In order to doodle more finely, an amplifier can be set up during the doodle process.

  * 画笔及形状

    画笔可以选择手绘、仿制、橡皮擦、文字、贴图，其中仿制功能跟PS中的类似，复制图片中的某处地方。形状可以选择手绘、箭头、直线、圆、矩形等。画笔的底色可以选择颜色，或者一张图片。

  * 撤销

    每一步的涂鸦操作都可以撤销。

  * 放缩、移动及旋转

    在涂鸦的过程中，可以自由地通过手势缩放和移动图片。支持旋转图片。

  * 放大器

    为了更细微地涂鸦，涂鸦过程中可以设置出现放大器。

# Usage 用法
  * A. Launch DoodleActivity directly (the layout is like demo images above). 使用写好的涂鸦界面，直接启动.启动的页面可参看上面的演示图片。
```java
DoodleParams params = new DoodleParams(); // 涂鸦参数
params.mImagePath = imagePath; // the file path of image
DoodleActivity.startActivityForResult(MainActivity.this, params, REQ_CODE_DOODLE);
```
See [DoodleParams](https://github.com/1993hzw/Doodle/blob/master/doodle/src/main/java/cn/hzw/doodle/DoodleParams.java) for more details.

  * B. Recommend, use DoodleView and customize your layout. 推荐的方法：使用DoodleView，自定义自己的交互界面.
```java
DoodleView mDoodle = mDoodleView = new DoodleView(this, bitmap, new IDoodleListener() {
            @Override
            public void onSaved(Bitmap bitmap, Runnable callback) { // called when save the doodled iamge. 保存涂鸦图像时调用
               //do something
            }

            @Override
            public void onError(int i, String msg) { // errors
               //do something
            }

            @Override
            public void onReady() { // called when it is ready to doodle because the view has been measured. Now, you can set size, color, pen, shape, etc. 此时view已经测量完成，涂鸦前的准备工作已经完成，在这里可以设置大小、颜色、画笔、形状等。
                //do something
            }
        });

mTouchGestureListener = new DoodleOnTouchGestureListener(mDoodleView, new DoodleOnTouchGestureListener.ISelectionListener() {
    @Override
    public void onSelectedItem(IDoodleSelectableItem selectableItem, boolean selected) { // called when the item(such as text, texture) is selected/unselected. item（如文字，贴图）被选中或取消选中时回调
        //do something
    }

    @Override
    public void onCreateSelectableItem(float x, float y) { // called when you click the view to create a item(such as text, texture). 点击View中的某个点创建可选择的item（如文字，贴图）时回调
        //do something
        /*
if (mDoodle.getPen() == DoodlePen.TEXT) {
        IDoodleSelectableItem item = new DoodleText(mDoodle, "hello", 20 * mDoodle.getSizeUnit(), new DoodleColor(Color.RED), x, y);
        mDoodle.addItem(item);
} else if (mDoodle.getPen() == DoodlePen.BITMAP) {
        IDoodleSelectableItem item = new DoodleBitmap(mDoodle, bitmap, 80 * mDoodle.getSizeUnit(), x, y);
        mDoodle.addItem(item);
}
        */
    }
});

// create touch detector, which dectects the gesture of scoll, scale, single tap, etc. 创建手势识别器，识别滚动，缩放，点击等手势
IDoodleTouchDetector detector = new DoodleTouchDetector(getApplicationContext(), mTouchGestureListener);
mDoodleView.setDefaultTouchDetector(detector);
```
Add the DoodleView to your layout. Now you can start freely. 把DoodleView添加到布局中，然后开始涂鸦。

DoodleView has implemented IDoodle. DoodleView实现了IDoodle接口。
```java
public interface IDoodle {
...
    public float getSizeUnit();
    public void setDoodleRotation(int degree);
    public void setDoodleScale(float scale, float pivotX, float pivotY);
    public void setPen(IDoodlePen pen);
    public void setShape(IDoodleShape shape);
    public void setDoodleTranslation(float transX, float transY);
    public void setSize(float paintSize);
    public void setColor(IDoodleColor color);
    public void addItem(IDoodleItem doodleItem);
    public void removeItem(IDoodleItem doodleItem);
    public void save();
    public void topItem(IDoodleItem item);
    public void bottomItem(IDoodleItem item);
    public boolean undo(int step);
...
}
```
```java
public enum DoodlePen implements IDoodlePen {
    BRUSH, // 画笔
    COPY, // 仿制
    ERASER, // 橡皮擦
    TEXT(true), // 文本
    BITMAP(true); // 贴图
...
}
```
```java
public enum DoodleShape implements IDoodleShape {
    HAND_WRITE, // 手绘
    ARROW, // 箭头
    LINE, // 直线
    FILL_CIRCLE, // 实心圆
    HOLLOW_CIRCLE, // 空心圆
    FILL_RECT, // 实心矩形
    HOLLOW_RECT; // 空心矩形
...
}
```

Now I think you should know that DoodleActivity has used DoodleView. You also can customize your layout like DoodleActivity. See [DoodleActivity](https://github.com/1993hzw/Doodle/blob/master/doodle/src/main/java/cn/hzw/doodle/DoodleActivity.java) for more details.

现在你应该知道DoodleActivity就是使用了DoodleView实现涂鸦，你可以参照[DoodleActivity](https://github.com/1993hzw/Doodle/blob/master/doodle/src/main/java/cn/hzw/doodle/DoodleActivity.java)是怎么实现涂鸦界面的交互来实现自己的自定义页面。

# Extend 拓展

You can create a customized item like DoodlePath, DoodleText, DoodleBitmap. Then call IDoodle.addItem(yourItem).

You can create a customize pen like DoodlePen which implements IDoodlePen.

You can create a customize shape like DoodleShape which implements IDoodleShape.

You can crate a customize touch gesture detector like DoodleTouchDetector which implements IDoodleTouchDetector.

For example, what should we do to achieve the Mosaic effect? Later...

# The developer 开发者

hzw19933@gmail.com

154330138@qq.com

Q&A <a target="_blank" href="//shang.qq.com/wpa/qunwpa?idkey=c79470c973e39f4f8e35da33fed431101354b67281766b6c12e9f310289d6c34"><img border="0" src="//pub.idqqimg.com/wpa/images/group.png" alt="Doodle-涂鸦交流群" title="Doodle-涂鸦交流群"></a>  6762102

Visit [My Blog](https://blog.csdn.net/u012964944) for more articles about Doodle.

欢迎大家访问[我的博客](https://blog.csdn.net/u012964944)，以便获取更多关于Doodle的文章哦.

# License

  ```
  MIT License
  
  Copyright (c) 2018 huangziwei
  
  Permission is hereby granted, free of charge, to any person obtaining a copy
  of this software and associated documentation files (the "Software"), to deal
  in the Software without restriction, including without limitation the rights
  to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
  copies of the Software, and to permit persons to whom the Software is
  furnished to do so, subject to the following conditions:
  
  The above copyright notice and this permission notice shall be included in all
  copies or substantial portions of the Software.
  
  THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
  IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
  FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
  AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
  LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
  OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
  SOFTWARE.
  ```