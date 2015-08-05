　　1 程序运行效果：

1.1程序启动第一个界面，等待用户选择倒计时时间


1.2用户选择好倒计时时间后，开始倒计时



1.3接受用户取消、暂停、继续等中断



 

　　2 设计过程

　　2.1 拖出来一个日期拾取器控件，两个按钮，关联属性变量，然后添加辅助属性变量，如下：

 1 @interface ViewController ()
 2 
 3 @property (weak, nonatomic) IBOutlet UIDatePicker *timePicker;
 4 @property (weak, nonatomic) IBOutlet UIButton *pauseAndContinueBtn;
 5 
 6 @property (weak, nonatomic) UILabel *timerLabel;
 7 @property (weak, nonatomic) NSTimer *countTimer;
 8 @property (assign, nonatomic) NSInteger second;
 9 
10 @end
　　2.2 开始计时按钮的响应函数为：

 1 - (IBAction)countStart:(UIButton *)countBtn {
 2     
 3     if([countBtn.titleLabel.text isEqualToString:@"开始计时"])
 4     {
 5         self.pauseAndContinueBtn.enabled = YES;
 6         
 7         UILabel *timeLabel = [[UILabel alloc] init];
 8         timeLabel.frame = self.timePicker.frame;
 9 
10         NSInteger second = self.second;
11         timeLabel.text = [NSString stringWithFormat:@"%02ld:%02ld:%02ld",second/3600,second%3600/60,second%60];
12         timeLabel.font = [UIFont systemFontOfSize:48];
13         timeLabel.textAlignment = NSTextAlignmentCenter;
14 
15         self.timePicker.alpha = 0.0f;
16         [self.view addSubview:timeLabel];
17 
18         self.timerLabel  =timeLabel;
19         [countBtn setTitle:@"取消" forState:UIControlStateNormal];
20 
21         self.countTimer = [NSTimer scheduledTimerWithTimeInterval:1.0f target:self selector:@selector(countStartSubForUpdateLabelText) userInfo:nil repeats:YES];
22         [[NSRunLoop mainRunLoop] addTimer:self.countTimer forMode:NSRunLoopCommonModes];
23     }
24     else
25     {
26         self.pauseAndContinueBtn.enabled = NO;
27         [self.pauseAndContinueBtn setTitle:@"暂停" forState:UIControlStateNormal];
28         
29         [self.timerLabel removeFromSuperview];
30         self.timePicker.alpha = 1.0f;
31         
32         [countBtn setTitle:@"开始计时" forState:UIControlStateNormal];
33         [self.countTimer invalidate];
34         
35         [self countDown:self.timePicker];
36     }
39 }
　　2.3 暂停继续按钮的响应函数为：

 1 - (IBAction)countPause:(UIButton *)pauseBtn
 2 {
 3     if([pauseBtn.titleLabel.text isEqualToString:@"暂停"])
 4     {
 5         [pauseBtn setTitle:@"继续" forState:UIControlStateNormal];
 6         [self.countTimer setFireDate:[NSDate distantFuture]];
 7     }
 8     else
 9     {
10         [pauseBtn setTitle:@"暂停" forState:UIControlStateNormal];
11         [self.countTimer setFireDate:[NSDate distantPast]];
12     }
13 }
　　2.4 日期拾取器将界面上用户选择的函数接收，并将结果写到一个存放秒数的属性变量中，代码如下：

 1 - (void)countDown:(UIDatePicker *)timePicker
 2 {
 3 
 4     NSDateFormatter *myDate = [[NSDateFormatter alloc] init];
 5     [myDate setDateFormat:@"HH:mm:ss"];
 6     NSString *stringTime = [myDate stringFromDate:timePicker.date];
 7     
 8     NSArray *hourAndMin = [stringTime componentsSeparatedByString:@":"];
 9     NSString *stringHour = hourAndMin[0];
10     NSString *stringMin = hourAndMin[1];
11     
12     NSInteger hour = stringHour.integerValue;
13     NSInteger min  = stringMin.integerValue;
14     NSInteger second = hour*3600+min*60;
15     
16     self.second = second;
17 
18 }
　　2.5 viewDidLoad函数中的加载内容：

1 - (void)viewDidLoad {
2     [super viewDidLoad];
3     
4     self.timePicker.datePickerMode = UIDatePickerModeCountDownTimer;
5     [self.timePicker addTarget:self action:@selector(countDown:) forControlEvents:UIControlEventValueChanged];
6     
7     self.pauseAndContinueBtn.enabled = NO;
8 }
　　2.6 倒计时label控件内容刷新函数：

1 - (void)countStartSubForUpdateLabelText
2 {
3     self.second--;
4     self.timerLabel.text = [NSString stringWithFormat:@"%02ld:%02ld:%02ld",self.second/3600,self.second%3600/60,self.second%60];
5 }
　　3 注意，日期拾取器控件属性设置为倒计时模式。



　　4 just have a try,you will find more…

 
