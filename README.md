# brave

使用NSTimer模仿iPhone 5s做的定时器界面
具备设置时间（通过datePickerView）、暂停和取消三个基本功能。
- (IBAction)countStart:(UIButton *)countBtn {
    
    if([countBtn.titleLabel.text isEqualToString:@"开始计时"])
    {
        self.pauseAndContinueBtn.enabled = YES;
        
        UILabel *timeLabel = [[UILabel alloc] init];
        timeLabel.frame = self.timePicker.frame;

        NSInteger second = self.second;
        timeLabel.text = [NSString stringWithFormat:@"%02ld:%02ld:%02ld",second/3600,second%3600/60,second%60];
        timeLabel.font = [UIFont systemFontOfSize:48];
        timeLabel.textAlignment = NSTextAlignmentCenter;

        self.timePicker.alpha = 0.0f;
        [self.view addSubview:timeLabel];

        self.timerLabel  =timeLabel;
        [countBtn setTitle:@"取消" forState:UIControlStateNormal];

        self.countTimer = [NSTimer scheduledTimerWithTimeInterval:1.0f target:self selector:@selector(countStartSubForUpdateLabelText) userInfo:nil repeats:YES];
        [[NSRunLoop mainRunLoop] addTimer:self.countTimer forMode:NSRunLoopCommonModes];
    }
    else
    {
        self.pauseAndContinueBtn.enabled = NO;
        [self.pauseAndContinueBtn setTitle:@"暂停" forState:UIControlStateNormal];
        
        [self.timerLabel removeFromSuperview];
        self.timePicker.alpha = 1.0f;
        
        [countBtn setTitle:@"开始计时" forState:UIControlStateNormal];
        [self.countTimer invalidate];
        
        [self countDown:self.timePicker];
    }

    
}
