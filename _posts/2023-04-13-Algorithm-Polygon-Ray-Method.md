---
layout: post
title:  "射线法判断点与多边形包含关系（凸/凹多边形均适用）"
categories: Algorithm
tags:  Algorithm Polygon
author: SHENYY
---

* content
{:toc}

## 1.算法原理
射线法判断一个点是否位于多边形内部的原理是，从待判断点发出一条向水平方向的射线，统计射线与多边形边界的交点数量。如果交点数量为奇数，则待判断点在多边形内部，否则在多边形外部。

具体来说，可以先将多边形边界按照某一顺序排序，然后从待判断点向水平方向发出一条射线，统计射线与多边形边界的交点数量。对于每条多边形边界，如果其与射线有交点，则交点数量加一。如果交点数量为奇数，则待判断点在多边形内部，否则在多边形外部。

射线法的优点是简单易懂，适用于凸多边形和凹多边形。但是需要注意，对于自交多边形，该方法可能会得到错误的结果。此时，更好的方法是使用其他算法，如扫描线算法或者三角剖分

<center><img src="https://shenyy1993.github.io/blog/assets/20230413-Algorithm-Polygon-Ray-Method-射线法示例.png" width="400" title="射线法示例"></center>




## 2.设计实现
流程图如下：
<center><img src="https://shenyy1993.github.io/blog/assets/20230413-Algorithm-Polygon-Ray-Method-流程图.png" width="600" title="流程图"></center>

## 3.Java代码实现
### 3维向量类Vector3
```java
public class Vector3 {
    private double iFactor;
    private double jFactor;
    private double kFactor;

    public Vector3(double i, double j, double k) {
        this.iFactor = i;
        this.jFactor = j;
        this.kFactor = k;
    }

    /**叉乘*/
    public Vector3 CrossProduct(Vector3 vector1, Vector3 vector2) {
        Vector3 result = new Vector3(0.0, 0.0, 0.0);
        double a1 = vector1.getiFactor();
        double a2 = vector1.getjFactor();
        double a3 = vector1.getkFactor();
        double b1 = vector2.getiFactor();
        double b2 = vector2.getjFactor();
        double b3 = vector2.getkFactor();
        result.setiFactor(a2 * b3 - a3 * b2);
        result.setjFactor(a3 * b1 - a1 * b3);
        result.setkFactor(a1 * b2 - a2 * b1);

        return result;
    }
}
```

### 多边形类Polygon
```java
public class Polygon {
    private List<Point2D> point;

    /**射线法判断点是否在多边形（凸/凹）范围内*/
    public static boolean pointInPolygon(List<Point2D> polygon, Point2D point) {
        int n = polygon.size();
        if (n < 3) return false;

        //获取最大的x值
        double xMax = point.getX() + 100;
        for (int i = 0; i < n; i++) {
            if (polygon.get(i).getX() > xMax) {
                xMax = polygon.get(i).getX() + 100;
            }
        }

        int count = 0;
        for (int i = 0; i < n; i++) {
            Point2D p1 = polygon.get(i);
            Point2D p2 = polygon.get((i + 1) % n);
            double sin = (point.getY() - p1.getY()) * (p2.getX() - p1.getX()) - (point.getX() - p1.getX()) * (p2.getY() - p1.getY());
            if (Math.abs(sin) < 1e-9) {// 如果sin值过小，则认为连线与边界平行，忽略该边界
                continue;
            }

            if ((p1.getY() < point.getY() && p2.getY() >= point.getY())
                    || (p2.getY() < point.getY() && p1.getY() >= point.getY())) {
                boolean cross = SegCross(point, new Point2D(xMax, point.getY()), p1, p2);
                if (cross) {
                    count++;
                }
            }
        }
        return count % 2 == 1;
    }
    
    /**
     * @author: shenyy
     * @methodsName: SegCross
     * @description: 两线段是否相交判断
     * @param: segA1 线段A点1
     * segA2 线段A点2
     * segB1 线段B点1
     * segB2 线段B点2
     * @return: crossFlag 相交标识符
     * @throws:
     */
    public static boolean SegCross(Point2D segA1, Point2D segA2, Point2D segB1, Point2D segB2) {
        boolean crossFlag = false;
        //三维向量构建
        Vector3 vecA1B1 = new Vector3(segB1.getX() - segA1.getX(), segB1.getY() - segA1.getY(), 0);
        Vector3 vecA2B1 = new Vector3(segB1.getX() - segA2.getX(), segB1.getY() - segA2.getY(), 0);
        Vector3 vecB2B1 = new Vector3(segB1.getX() - segB2.getX(), segB1.getY() - segB2.getY(), 0);

        Vector3 vecB1A1 = new Vector3(segA1.getX() - segB1.getX(), segA1.getY() - segB1.getY(), 0);
        Vector3 vecB2A1 = new Vector3(segA1.getX() - segB2.getX(), segA1.getY() - segB2.getY(), 0);
        Vector3 vecA2A1 = new Vector3(segA1.getX() - segA2.getX(), segA1.getY() - segA2.getY(), 0);

        //叉乘计算,点乘计算
        Vector3 vecCross1 = new Vector3(0.0, 0.0, 0.0);
        Vector3 vecCross2 = new Vector3(0.0, 0.0, 0.0);
        vecCross1 = vecCross1.CrossProduct(vecA1B1, vecB2B1);
        vecCross2 = vecCross2.CrossProduct(vecA2B1, vecB2B1);
        double dotResult1 = vecCross1.getiFactor() * vecCross2.getiFactor() + vecCross1.getjFactor() * vecCross2.getjFactor() + vecCross1.getkFactor() * vecCross2.getkFactor();

        vecCross1 = vecCross1.CrossProduct(vecB1A1, vecA2A1);
        vecCross2 = vecCross2.CrossProduct(vecB2A1, vecA2A1);
        double dotResult2 = vecCross1.getiFactor() * vecCross2.getiFactor() + vecCross1.getjFactor() * vecCross2.getjFactor() + vecCross1.getkFactor() * vecCross2.getkFactor();

        //判断线段是否相交
        if (dotResult1 < 0 && dotResult2 < 0) {
            crossFlag = true;
        }

        //返回结果
        return crossFlag;
    }
}
```

## 测试

在多边形附近区域随机生成100个点测试，测试代码如下，
```java
class TestClass{
    public static void main(String[] args) {
        //测试pointInPolygon
        Point2D[] points = new Point2D[6];
        points[0] = new Point2D(0, 0);
        points[1] = new Point2D(2, 0);
        points[2] = new Point2D(2, 1);
        points[3] = new Point2D(1, 1);
        points[4] = new Point2D(1, 2);
        points[5] = new Point2D(0, 2);

        List<Point2D> pointList = new ArrayList<>();
        for (int i = 0; i < points.length; i++) {
            pointList.add(points[i]);
        }

        for (int i = 0; i < 100; i++) {
            Point2D testPoint = new Point2D(6 * Math.random() - 2, 6 * Math.random() - 2);
            System.out.println(testPoint + ": " + pointInPolygon(points, testPoint));
        }
    }   
}
```
测试结果如下，绿点代表包含；红点代表不包含。
<center><img src="https://shenyy1993.github.io/blog/assets/20230413-Algorithm-Polygon-Ray-Method-测试结果1.png" width="400" title="测试结果1"></center>



