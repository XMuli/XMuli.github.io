---
title: QTableWidget表格组件的属性介绍和使用
date: 2019-12-11 00:04:06
toc: true
categories: 
 - [学习 - qt]
 - [专栏 - Qt推倒重学系列]
---



**简  述：**  `QTableWidget`表格，组件的属性介绍和使用，这里写一个简单小例子的使用介绍它的基本属性的使用

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/Snipaste_2019-12-08_22-42-50_mark.png"/>

<!-- more -->

[TOC]

## 开发环境：

**编程环境：**  `MacOS 10.14.6 (18G103)`   **编程软件：** `Qt 5.9.8`， `Qt Creator 4.8.2`

<br>

## QTableWidget属性：

`QTableWidget`是 **表格组件** ；其属于显示窗口的控件，只是将数据以表格的形式展示出来， 

其主要属性是：

- row行

行号

- Colum列

列号

- Item项

`QTableWidgetItem`是具体的某一个项item，就是表格中间的具体一个项（小长方块）；还可以用来存储数据（但是在界面该数据是不显示的），每一个item都可以存储一个自己的`QVariant`数据；

<br>

## 运行效果：

来一张运行效果图：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/2019-12-08-22-56-38.gif"/>

<br>

## 核心源码讲解：

因为这个控件属于比较简单，所以此处直接将部分**.h**源码贴出来:

```cpp
class ExQTableWidget : public QMainWindow
{

...
    void createItemsARow(int row, QString name, QString sex, QDate birth, QString nation, int score, bool isAnime);   //为某一行创建items

private slots:
    void on_btnSetHeader_clicked();                      //设置表头
    void on_btnInitTable_clicked();                      //初始化数据表
    void on_btnSetRow_clicked();                         //设置表格的行数
    void on_btnInsertRow_clicked();                      //插入行
    void on_btnAddRow_clicked();                         //在最后一行添加一行
    void on_btnDelRow_clicked();                         //删除当前行
    void on_btnAutoHeight_clicked();                     //自动设置行高
    void on_btnAutoWidth_clicked();                      //自动设置列高
    void on_btnReadToEdit_clicked();                     //读取表格内容到文本
    void on_chkBoxHeadEdit_clicked(bool checked);        //表格可编辑模式
    void on_chkBoxRowColor_clicked(bool checked);        //间隔行底色
    void on_chkBoxHeadRow_clicked(bool checked);         //显示行表头
    void on_chkBoxHeadCol_clicked(bool checked);         //显示列表头
    void on_radioBtnSelectItem_clicked();                //单元格选择
    void on_radioBtnSelectRow_clicked();                 //行选择
    void on_tableWidget_currentCellChanged(int currentRow, int currentColumn, int previousRow, int previousColumn);  //获取当前item和前一个(点击)item的行列号

private:
    QLabel *m_labCellIndex;                              //状态栏上用来显示单元格的行号、列号
    QLabel *m_labCellType;                               //状态栏上用来显示单元格的type
    QLabel *m_labStudID;                                 //状态栏上用来显示单元格的学号
};
```

其对应的**.cpp**文件：

```cpp
ExQTableWidget::ExQTableWidget(QWidget *parent) :
    QMainWindow(parent),
    ui(new Ui::ExQTableWidget)
{
    ui->setupUi(this);
    setWindowTitle(QObject::tr("QTableWidget的讲解和使用"));

    m_labCellIndex = new QLabel("当前单元格坐标：", this);          //状态栏初始化
    m_labCellIndex->setMinimumWidth(250);
    m_labCellType = new QLabel("当前单元格类型：", this);
    m_labCellType->setMinimumWidth(200);
    m_labStudID = new QLabel("学生ID：", this);
    m_labStudID->setMinimumWidth(200);
    ui->statusBar->addWidget(m_labCellIndex);
    ui->statusBar->addWidget(m_labCellType);
    ui->statusBar->addWidget(m_labStudID);
    ui->chkBoxHeadEdit->setChecked(Qt::Unchecked);

    setCentralWidget(ui->splitter_2);
    on_btnSetHeader_clicked();
}

//为某一行创建items
void ExQTableWidget::createItemsARow(int row, QString name, QString sex, QDate birth, QString nation, int score, bool isAnime)
{
    QTableWidgetItem* item = nullptr;
    int stunID = 20190913;
    QString str = "";

    item = new QTableWidgetItem(name, cellType::ctName);            //创建name 坐标为(0, 0)的item
    item->setTextAlignment(Qt::AlignHCenter | Qt::AlignVCenter);    //文本显示居中
    stunID += row;                                                  //学号 = 基数 + 行号
    item->setData(Qt::UserRole, QVariant(stunID));                  //设置stunID为data
    ui->tableWidget->setItem(row, fieldColNum::colName, item);      //插入item到tableWidget里面

    item = new QTableWidgetItem(sex, cellType::ctSex);              //性别
    QIcon icon;
    if (sex == "男")
        icon.addFile(":/images/Image002.ico");
    else
        icon.addFile(":/images/Image003.ico");
    item->setIcon(icon);
    item->setTextAlignment(Qt::AlignHCenter | Qt::AlignVCenter);
    ui->tableWidget->setItem(row, fieldColNum::colSex, item);

    item = new QTableWidgetItem(birth.toString("yyyy-MM-dd"), cellType::ctBirth);//出生日期
    item->setTextAlignment(Qt::AlignHCenter | Qt::AlignVCenter);
    ui->tableWidget->setItem(row, fieldColNum::colBirth, item);

    item = new QTableWidgetItem(nation, cellType::ctNation);          //籍贯
    item->setTextAlignment(Qt::AlignHCenter | Qt::AlignVCenter);
    ui->tableWidget->setItem(row, fieldColNum::colNation, item);

    item = new QTableWidgetItem(str.setNum(score), cellType::ctScore);//成绩
    item->setTextAlignment(Qt::AlignHCenter | Qt::AlignVCenter);
    ui->tableWidget->setItem(row, fieldColNum::colScore, item);

    item = new QTableWidgetItem("喜欢", cellType::ctLikeAnime);       //喜欢二次元否?
    if (isAnime)
        item->setCheckState(Qt::Checked);
    else
        item->setCheckState(Qt::Unchecked);
    item->setTextAlignment(Qt::AlignHCenter | Qt::AlignVCenter);
    item->setBackgroundColor(Qt::gray);
    ui->tableWidget->setItem(row, fieldColNum::colLikeAnime, item);
}

//+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
//设置表头
void ExQTableWidget::on_btnSetHeader_clicked()
{
    QTableWidgetItem* item = nullptr;
    QStringList list;
    list << "姓名" << "性别" << "出生日期" << "籍贯" << "分数" << "是否喜欢二次元";

//    ui->tableWidget->setHorizontalHeaderLabels(list);            //只是初始化表头的每一列的名字
    ui->tableWidget->setColumnCount(list.count());

    for (int i = 0; i < ui->tableWidget->columnCount(); i++) {
        item = new QTableWidgetItem(list.at(i));                   //创建一个item
        QFont font;
        font.setBold(true);
        font.setPointSize(12);
        item->setFont(font);                                       //设置item的字体，颜色，粗体，大小
        item->setTextColor(Qt::red);
        ui->tableWidget->setHorizontalHeaderItem(i, item);         //添加item到tableWidget
    }
}

//初始化数据表
void ExQTableWidget::on_btnInitTable_clicked()
{
    QTime time;
    time = QTime::currentTime();
    qsrand(static_cast<uint>(time.msec() + time.second() * 1000));

    QString name = "学生";
    QString sex = "保密";
    QDate birth(2019, 9, 12);
    QString nation = "武汉";
    int score = 87;
    bool isAnime = true;

    ui->tableWidget->clearContents();                              //清除工作区，不清除表头
    int row = ui->tableWidget->rowCount();

    for (int i = 0; i < row; i++) {                                //初始化一行行的数据
        name = "学生";
        name += QString::number(i);
        int n  = qrand() % 2;                                      //产生随机数小于2

        if (n == 0) {
            sex = "男";
            isAnime = true;
        }
        else {
            sex = "女";
            isAnime = false;
        }

        score += 10;
        birth = birth.addDays(30);
        createItemsARow(i, name, sex, birth, nation, score, isAnime);
    }
}

//设置表格的行数
void ExQTableWidget::on_btnSetRow_clicked()
{
    int n = ui->spinBoxRow->value();
    ui->tableWidget->setRowCount(n);
}

//插入行
void ExQTableWidget::on_btnInsertRow_clicked()
{
    int currRow = ui->tableWidget->currentRow();
    ui->tableWidget->insertRow(currRow);                           //插入一行，不会自动为单元格创建item
    createItemsARow(currRow, "插入学生", "女", QDate(2020, 07, 17), "楚国", 87, true);
}

//在最后一行添加一行
void ExQTableWidget::on_btnAddRow_clicked()
{
    int row = ui->tableWidget->rowCount();
     ui->tableWidget->insertRow(row);
    createItemsARow(row, "添加学生", "女", QDate(2021, 07, 17), "唐朝", 93, true);
}

//删除当前行
void ExQTableWidget::on_btnDelRow_clicked()
{
    int currRow = ui->tableWidget->currentRow();
    ui->tableWidget->removeRow(currRow);                           //删除当前行以及其他items
}

//自动设置行高
void ExQTableWidget::on_btnAutoHeight_clicked()
{
    ui->tableWidget->resizeRowsToContents();                       //自动调整所有行的内容，以适应内容高度
}

//自动设置列高
void ExQTableWidget::on_btnAutoWidth_clicked()
{
    ui->tableWidget->resizeColumnsToContents();                    //自动调整所有列的内容，以适应内容宽度
}

//读取表格内容到文本
void ExQTableWidget::on_btnReadToEdit_clicked()
{
    QTableWidgetItem* item = nullptr;
    QString str = "";
    for (int i = 0; i < ui->tableWidget->rowCount(); i++) {
        str = "";
        for (int j = 0; j < ui->tableWidget->columnCount() - 1; j++) {
            item = ui->tableWidget->item(i, j);
            str += item->text() + "  ";
        }

        item = ui->tableWidget->item(i, fieldColNum::colLikeAnime);

        if (item->checkState() == Qt::Checked)
            str += "喜欢二次元";
        else
            str += "不喜欢二次元";

        ui->plainTextEdit->appendPlainText(str);
    }

}

//表格可编辑模式
void ExQTableWidget::on_chkBoxHeadEdit_clicked(bool checked)
{
    if (checked)
        ui->tableWidget->setEditTriggers(QAbstractItemView::DoubleClicked | QAbstractItemView::SelectedClicked);
    else
        ui->tableWidget->setEditTriggers(QAbstractItemView::DoubleClicked);
}

//间隔行底色
void ExQTableWidget::on_chkBoxRowColor_clicked(bool checked)
{
    ui->tableWidget->setAlternatingRowColors(checked);
}

//显示行表头
void ExQTableWidget::on_chkBoxHeadRow_clicked(bool checked)
{
    ui->tableWidget->horizontalHeader()->setVisible(checked);
}

//显示列表头
void ExQTableWidget::on_chkBoxHeadCol_clicked(bool checked)
{
    ui->tableWidget->verticalHeader()->setVisible(checked);
}

//单元格选择
void ExQTableWidget::on_radioBtnSelectItem_clicked()
{
    ui->tableWidget->setSelectionBehavior(QAbstractItemView::SelectItems);
}

//行选择
void ExQTableWidget::on_radioBtnSelectRow_clicked()
{
    ui->tableWidget->setSelectionBehavior(QAbstractItemView::SelectRows);
}

//获取当前item和前一个(点击)item的行列号
void ExQTableWidget::on_tableWidget_currentCellChanged(int currentRow, int currentColumn, int previousRow, int previousColumn)
{
    m_labCellIndex->setText("当前单元格坐标：[" + QString::number(currentRow) + "行, " + QString::number(currentColumn) + "列]");
    QTableWidgetItem* item = ui->tableWidget->item(currentRow, currentColumn);
    int type = item->type();
    m_labCellType->setText("当前单元格类型：" + QString::number(type));
    QTableWidgetItem* item2 = ui->tableWidget->item(currentRow, fieldColNum::colName);
    m_labStudID->setText("学生ID：" + item2->data(Qt::UserRole).toString());
}
```

<br>

## 源码下载：

[https://github.com/xmuli/QtExamples](https://github.com/xmuli/QtExamples) 【QtQTableWidgetEx】

