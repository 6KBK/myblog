**项目十 各部门工资最高的员工** 这题思路比较简单，就是先查询出每个部门最高的工资，然后再通过JOIN匹配部门信息
![图片](https://uploader.shimo.im/f/9TSIgkUydF4WWM0q!thumbnail)
**项目十一 换座位**从最后结果来看就是 ：
①id为偶数的需要往前挪
②id为奇数的需要往后挪
③再考虑最后一位是奇数还是偶数，奇数不变（发现偶数的情况已经包含在前面了）

通过 id%2=1 来判断是奇数。
这里需要注意的地方是，最后一位，我们在判断奇数往后挪的时候，是不包含最后一位的。所以在②的时候要限定 id 小于总个数。
![图片](https://uploader.shimo.im/f/z3DYFQLDxDMtMkZ3!thumbnail)
**项目十二 分数排名**
这里的排名是连续排名。将分数去重后的清单排名就是最终排名。然后将这个排名JOIN原始数据。
问题在于，怎么在去重的分数清单增加一列ID，这个ID会随着数据自动增加。这个可以通过参数化查询搞定，但是我们还没学，所以pass。
然后就有个取巧的办法。我们还是需要获取去重的分数清单，但是不需要排名ID了：
①  原始表里的最大值。 分数清单只取最大值，然后COUNT(*)，这样就只有1
②  原始表里的第二大值。 分数清单取最大值和第二大值，然后COUNT(*), 这样就是2
```
以此内推。
在code里就是 分数清单里的score要大于等于原始表的某个具体的值。

这个写法的缺点是，每一个原始表socre列的值，都需要重新扫描去重后的分数清单，在数据表大的情况下，性能会很差。
![图片](https://uploader.shimo.im/f/U8V5mwjsmLA3VHkF!thumbnail)


# 项目十三：连续出现的数字

# 项目十四：树节点
对于一个节点，先判断是否为根节点。如果不是，再判断是内部节点还是叶子节点。
如果一个节点的父节点id为空（null），则为根节点：
| ** 12 **   | ** SELECT id, IF(ISNULL(p_id), 'Root', IF()) AS Type FROM tree; **   | 
|----|----|
对于一个不是根节点的节点，如果它是某些节点的父节点，则为内部节点，否则为叶子节点：
| ** 12 **   | ** SELECT id, IF(ISNULL(p_id), 'Root', IF(id IN (SELECT p_id FROM tree), 'Inner', 'Leaf')) AS Type FROM tree; **   | 
|----|----|
最后，根据id排序：
| ** 12 **   | ** SELECT id, IF(ISNULL(p_id), 'Root', IF(id IN (SELECT p_id FROM tree), 'Inner', 'Leaf')) AS Type FROM tree ORDER BY id; **   | 
|----|----|
# 项目十五：至少有五名直接下属的经理
可以根据ManagerId分组，找出有5个下属的主管的Id：
| ** 1 **   | ** SELECT ManagerId FROM Employee GROUP BY ManagerId HAVING COUNT(*) > 4; **   | 
|----|----|
然后输出对应的姓名：
| ** 12 **   | ** SELECT Name FROM Employee WHERE Id IN (SELECT ManagerId FROM Employee GROUP BY ManagerId HAVING COUNT(*) > 4); **   | 
|----|----|
或者，把两张**Employee**表连接（join）起来，一张作为员工，一张作为主管。然后根据主管分组，并筛选出符合条件的组：
| ** 123 **   | ** SELECT m.Name FROM Employee AS e JOIN Employee AS m ON e.ManagerId = m.Id GROUP BY m.Name HAVING COUNT(e.Name) >= 5; **   | 
|----|----|

