# flutter_story_ui
Youtube上看到的视频，跟着学习一下堆叠布局的使用, 如果觉得有帮助，留个star再走叭
#### 实现效果
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200426192048101.gif)
#### 滑动效果

* 使用PageView滑动来控制当前显示的位置
```
Stack(
  children: <Widget>[
    // 两者堆叠在一起。通过PageView滑动的Controller来控制当前显示的page
    CardScrollWidget(currentPage),
    Positioned.fill(
      child: PageView.builder(
        itemCount: images.length,
        controller: controller,
        reverse: true,
        itemBuilder: (context, index) {
          return Container();
        },
      ),
    )
  ],
),
```
#### CardScrollWidget
```
class CardScrollWidget extends StatelessWidget {
  var currentPage;
  var padding = 20.0;
  var verticalInset = 20.0;
  CardScrollWidget(this.currentPage);
  @override
  Widget build(BuildContext context) {
    return AspectRatio(
      aspectRatio: (12.0 / 16.0) * 1.2,
      child: LayoutBuilder(
        builder: (context, contraints) {
          var width = contraints.maxWidth;
          var height = contraints.maxHeight;
          var safeWidth = width - 2 * padding;
          var safeHeight = height - 2 * padding;
          var heightOfPrimaryCard = safeHeight;
          var widthOfPrimaryCard = heightOfPrimaryCard * 12 / 16;
          var primaryCardLeft = safeWidth - widthOfPrimaryCard;
          var horizontalInset = primaryCardLeft / 2;
          List<Widget> cardList = List();
          for (var i = 0; i < images.length; i++) {
            var leftPage = i - currentPage;
            bool isOnRight = leftPage > 0;
            var start = padding +
                max(
                    primaryCardLeft -
                        horizontalInset * -leftPage * (isOnRight ? 15 : 1),
                    0);
            var cardItem = Positioned.directional(
                top: padding + verticalInset * max(-leftPage, 0.0),
                bottom: padding + verticalInset * max(-leftPage, 0.0),
                start: start,
                textDirection: TextDirection.rtl,
                child: ClipRRect(
                  borderRadius: BorderRadius.circular(16.0),
                  child: Container(
                    decoration: BoxDecoration(color: Colors.white, boxShadow: [
                      BoxShadow(
                          color: Colors.black12,
                          offset: Offset(3.0, 6.0),
                          blurRadius: 10.0)
                    ]),
                    child: AspectRatio(
                      aspectRatio: 12 / 16,
                      child: Stack(
                        fit: StackFit.expand,
                        children: <Widget>[
                          Image.asset(images[i], fit: BoxFit.cover),
                          Align(
                            alignment: Alignment.bottomLeft,
                            child: Column(
                              mainAxisSize: MainAxisSize.min,
                              crossAxisAlignment: CrossAxisAlignment.start,
                              children: <Widget>[
                                // 设置标题
                                Padding(
                                  padding: EdgeInsets.symmetric(
                                      horizontal: 16, vertical: 8),
                                  child: Text(
                                    title[i],
                                    style: TextStyle(
                                      color: Colors.white,
                                      fontSize: 25,
                                    ),
                                  ),
                                ),
                                SizedBox(
                                  height: 10,
                                ),
                                // 设置ReaderLater
                                Padding(
                                  padding:
                                      EdgeInsets.only(left: 12, bottom: 12),
                                  child: Container(
                                    padding: EdgeInsets.symmetric(
                                        horizontal: 22.0, vertical: 6.0),
                                    decoration: BoxDecoration(
                                        color: Colors.blueAccent,
                                        borderRadius:
                                            BorderRadius.circular(20.0)),
                                    child: Text("点击查看",
                                        style: TextStyle(color: Colors.white)),
                                  ),
                                )
                              ],
                            ),
                          )
                        ],
                      ),
                    ),
                  ),
                ));
            cardList.add(cardItem);
          }
          return Stack(
            children: cardList,
          );
        },
      ),
    );
  }
}
```
