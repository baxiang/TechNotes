当我们调用addChildViewController方法后，必须调用didMoveToParentViewController方法。 [子视图控制器 didMoveToParentViewController:父视图控制器] 当调用removeFromParentViewController方法前，必须先调用willMoveToParentViewController方法，且parent参数为nil： [将要删除的视图控制器 willMoveToParentViewController:nil]; 当从一个视图控制容器中添加或者移除viewController后，该方法被调用。
```
-(void)willMoveToParentViewController:(UIViewController *)parent
-(void)didMoveToParentViewController:(UIViewController *)parent
```
parent：父视图控制器，添加时parent为父视图控制器，删除时parent为nil

在调用addChildViewController之后，必需调用didMoveToParentViewController:parentViewController:parentVC(若不调用该方法，chileVC中的didMoveToParent方法不会自动调用)。willMoveToParentViewController默认调用了。
```
#pragma mark - 添加子控制器
- (void)addContentController:(UIViewController*)newVC
{
    [self addChildViewController:newVC];
    [self.view addSubview:newVC.view];
    [newVC didMoveToParentViewController:self];//必需调用这句 willMove:self由系统调用
}
```

```
#pragma mark - 删除子控制器
- (void)deleteContentController: (UIViewController*)viewController {
    [viewController willMoveToParentViewController:nil];//删除前必需调用，系统默认调用didMove:nil
    [viewController.view removeFromSuperview];
    [viewController removeFromParentViewController];
}
```
```
#pragma mark - 切换子控制器(切换)
- (void)changeFromViewController: (UIViewController*)oldVC
                toViewController: (UIViewController*)newVC {

    [self transitionFromViewController: oldVC toViewController: newVC
                              duration: 0.25 options:0
                            animations: Nil
                            completion: Nil];
}
```
