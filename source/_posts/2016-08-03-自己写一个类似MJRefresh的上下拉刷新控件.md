---
layout: post
title: "制作自己的下拉刷新动画"
categories: iOS开发
keywords: iOS开发, 下拉刷新
tags:
  - iOS
  - Git
---

实现原理:

其实仔细想想, 上下拉刷新的原理还是很简单的 ------>>> 首先把刷新控件添加到scrollView的头部或者底部, 然后监控到scrollView的滚动进度(底部刷新控件还需要监控scrollView的内容的改变, 每次改变后再次将控件调整到scrollView的底部), 根据不同的进度来设置刷新控件的相应的文字和图片动画等...

实现过程:

首先写一个scrollView的分类, 在分类中给scrollView添加两个属性zj_refreshHeader和zj_refreshFooter用来存取header和footer刷新控件, 这里有两种方法可以实现
1, 使用运行时

private var ZJHeaderKey: UInt8 = 0
private var ZJFooterKey: UInt8 = 0
extension UIScrollView {
    private var zj_refreshHeader: RefreshView? {
        set {
            objc_setAssociatedObject(self, &ZJHeaderKey, newValue, .OBJC_ASSOCIATION_RETAIN_NONATOMIC)
        }
        get {
            return objc_getAssociatedObject(self, &ZJHeaderKey) as? RefreshView
        }
    }
    private var zj_refreshFooter: RefreshView? {
        set {
            objc_setAssociatedObject(self, &ZJFooterKey, newValue, .OBJC_ASSOCIATION_RETAIN_NONATOMIC)
        }
        get {
            return objc_getAssociatedObject(self, &ZJFooterKey) as? RefreshView
        }
    }
}
2, 使用tag来存取

private var ZJHeaderTag = 1994
private var ZJFooterTag = 1995
extension UIScrollView {
    private var zj_refreshHeader: RefreshView? {
        set {
            if let header = newValue {
                header.tag = ZJHeaderTag
                addSubview(header)
            }
        }
        get {
            return viewWithTag(ZJHeaderTag) as? RefreshView
        }
    }
    private var zj_refreshFooter: RefreshView? {
        set {
            if let footer = newValue {
                footer.tag = ZJFooterTag
                addSubview(footer)
            }
        }
        get {
            return viewWithTag(ZJFooterTag) as? RefreshView
        }
    }
 }
然后在分类中给出使用header和footer的方法, 注意看, 这里我使用了一点swift中强大的泛型和类型约束, 这个就是约束Animator必须是UIView并且遵守RefreshViewDelegate协议的类型
public func zj_addRefreshHeader(headerAnimator: Animator, refreshHandler: RefreshHandler ) {
}
public func zj_addRefreshFooter(footerAnimator: Animator, refreshHandler: RefreshHandler ) {
}
接着提供开启和结束刷新动画的方法

/// 开启header刷新
public func zj_startHeaderAnimation() {
   zj_refreshHeader?.canBegin = true
}
/// 结束header刷新
 public func zj_stopHeaderAnimation() {
   zj_refreshHeader?.canBegin = false
}
/// 开启footer刷新
public func zj_startFooterAnimation() {
   zj_refreshFooter?.canBegin = true
}
/// 结束footer刷新
public func zj_stopFooterAnimation() {
   zj_refreshFooter?.canBegin = false
}
然后是RefreshView的实现, 在笔者的实现中, RefreshView是添加到scrollView的顶部或者底部来作为真正的刷新控件的容器

刷新控件的状态: 实际上控件有四种状态

public enum RefreshViewState {
    /// 正在加载状态
    case loading
    /// 正常状态
    case normal
    /// 下拉状态
    case pullToRefresh
    /// 松开手即进入刷新状态
    case releaseToFresh
}
1, 正常状态, 即未开始和已经结束的状态.

2, 拖拽状态, 这个时候拖拽的进度小于1, 如果继续拖拽直到拖拽进度等于(>)1的时候, 进入下一种状态.

3, 松手即进入刷新的状态, 这个时候松开手才能进入下一个状态, 如果不松开手, 向反方向拖拽, 则拖拽进度会减小, 如果进度<1, 则会进入上一个状态 ...

4, 加载动画状态, 这个时候进入加载状态, 知道收到 结束动画的指定, 才结束刷新动画进入正常状态等待

下拉刷新

首先将刷新控件添加到scrollView的顶部(在scrollView的分类方法中添加)

///
public func zj_addRefreshHeader(headerAnimator: Animator, refreshHandler: RefreshHandler ) {
    if let header = zj_refreshHeader {
        header.removeFromSuperview()
    }
    ///
    let frame = CGRect(x: 0.0, y: -headerAnimator.bounds.height, width: bounds.width, height: headerAnimator.bounds.height)
    zj_refreshHeader = RefreshView(frame: frame, refreshType: .header, refreshAnimator: headerAnimator, refreshHandler: refreshHandler)
    addSubview(zj_refreshHeader!)
}
然后需要监控scrollView的滚动(利用Cocoa强大的kvo机制)

private func addObserverOf(scrollView: UIScrollView?) {
     scrollView?.addObserver(self, forKeyPath: ConstantValue.ScrollViewContentOffsetPath, options: .Initial, context: &ConstantValue.RefreshViewContext)
}
1470131607504871.png

在scrollView的滚动过程中, 根据滚动的偏移量来计算出拖拽的进度, 然后计算出对应的header的状态, 根据不同的状态来相应的调整不同的UI或者动画

if scrollView.contentOffset.y > -scrollViewOriginalValue.contentInset.top {/**头部视图(隐藏)并且还没到显示的临界点*/ return }
// 已经进入拖拽状态, 进行相关操作
let progress = (-scrollViewOriginalValue.contentInset.top - scrollView.contentOffset.y) / self.bounds.height
if scrollView.tracking {
  if progress >= 1.0 {
          refreshViewState = .releaseToFresh
      } else if progress <= 0.0 {
           refreshViewState = .normal
     } else {
           refreshViewState = .pullToRefresh
         }
    }
 else if refreshViewState == .releaseToFresh {// releaseToFreah 2 refresh
         canBegin = true// begin refresh
  }
  else {// release
       if progress <= 0.0 {
           refreshViewState = .normal
       }
 }
 var actualProgress = min(1.0, progress)
actualProgress = max(0.0, actualProgress)
 refreshAnimator.refreshDidChangeProgress(self, progress: actualProgress, refreshViewType: refreshViewType)
开始和停止动画的处理, 这个时候需要调整scrollView的contentInset ----> 注意这里需要了解scrollView的三大属性 contentInset, contentOffset, contentSize (这里就省略介绍了)

开始动画的时候, 因为刷新控件是添加到scrollView的头部或者底部的, 在滚动的时候因为scrollView的bounces的原因, 松开手之后, 刷新控件是会回到原来的位置的, 这个时候, 我们希望加载动画的时候, 刷新控件停在我们的实现之内, 所以需要调整scrollView的contentInset(会自动调整contentOffset), 比如下拉刷新需要将contentInset的top加上刷新控件的高度, 上拉刷新的时候需要将contentInset的bottom加上刷新控件的高度

private func startAnimation() {
        guard let validScrollView = scrollView else { return }
        validScrollView.bounces = false
        /// may update UI
        dispatch_async(dispatch_get_main_queue(), {[weak self] in
            guard let validSelf = self else { return }
            UIView.animateWithDuration(0.25, animations: {
                if validSelf.refreshViewType == .header {
                    validScrollView.contentInset.top = validSelf.scrollViewOriginalValue.contentInset.top + validSelf.bounds.height
                } else {
                    let offPartHeight = validScrollView.contentSize.height - validSelf.heightOfContentOnScreenOfScrollView(validScrollView)
                    /// contentSize改变的时候设置的self.y不同导致不同的结果
                    /// 所有内容高度>屏幕上显示的内容高度
                    let notSureBottom = validSelf.scrollViewOriginalValue.contentInset.bottom + validSelf.bounds.height
                    validScrollView.contentInset.bottom = offPartHeight>=0 ? notSureBottom : notSureBottom - offPartHeight // 加上
                }
                }, completion: { (_) in
                    /// 这个时候才正式刷新
                    validScrollView.bounces = true
                    validSelf.refreshViewState = .loading
                    validSelf.refreshHandler()
            })
            })
    }
停止动画的时候, 需要将scrollView的contentInset复原为动画开始之前, 以便于不影响页面的其他布局

对于上拉刷新而言, 只是要多一个监控scrollView的contentSize, 在其改变的时候再次将刷新控件调整到scrollView的contentSize的底部

RefreshViewDelegate的定义
public protocol RefreshViewDelegate {
    /// 你应该为每一个header或者footer设置一个不同的key来保存时间, 否则将公用同一个key使用相同的时间
    var lastRefreshTimeKey: String? { get }
    /// 是否刷新完成后自动隐藏 默认为false
    var isAutomaticlyHidden: Bool { get }
    /// 上次刷新时间, 有默认赋值和返回
    var lastRefreshTime: NSDate? { get set }
    /// repuired 三个必须实现的代理方法
    /// 开始进入刷新(loading)状态, 这个时候应该开启自定义的(动画)刷新
    func refreshDidBegin(refreshView: RefreshView, refreshViewType: RefreshViewType)
    /// 刷新结束状态, 这个时候应该关闭自定义的(动画)刷新
    func refreshDidEnd(refreshView: RefreshView, refreshViewType: RefreshViewType)
    /// 刷新状态变为新的状态, 这个时候可以自定义设置各个状态对应的属性
    func refreshDidChangeState(refreshView: RefreshView, fromState: RefreshViewState, toState: RefreshViewState, refreshViewType: RefreshViewType)
    /// optional 两个可选的实现方法
    /// 允许在控件添加到scrollView之前的准备
    func refreshViewDidPrepare(refreshView: RefreshView, refreshType: RefreshViewType)
    /// 拖拽的进度, 可用于自定义实现拖拽过程中的动画
    func refreshDidChangeProgress(refreshView: RefreshView, progress: CGFloat, refreshViewType: RefreshViewType)
}
最后是自己继承 RefreshViewDelegate实现自定义的加载, 这里, 笔者提供了两种使用实例(代码布局和xib), 这两种能够完成MJRefresh提供的使用效果, 当然, 更灵活的自定义方式, 你可以自己随意实现, 具体的你可以参见demo中的示例, 这里只贴一点代码出来

public class NormalAnimator: UIView {
    /// 设置imageView
    @IBOutlet private(set) weak var imageView: UIImageView!
    @IBOutlet private(set) weak var indicatorView: UIActivityIndicatorView!
    /// 设置state描述
    @IBOutlet private(set) weak var descriptionLabel: UILabel!
    /// 上次刷新时间label footer 默认为hidden, 可设置hidden=false开启
    @IBOutlet private(set) weak var lastTimelabel: UILabel!
    public typealias SetDescriptionClosure = (refreshState: RefreshViewState, refreshType: RefreshViewType) -> String
    public typealias SetLastTimeClosure = (date: NSDate) -> String
    /// 是否刷新完成后自动隐藏 默认为false
    /// 这个属性是协议定义的, 当写在class里面可以供外界修改, 如果写在extension里面只能是可读的
    public var isAutomaticlyHidden: Bool = false
    private var setupDesctiptionClosure: SetDescriptionClosure?
    private var setupLastTimeClosure: SetLastTimeClosure?
    /// 耗时
    private lazy var formatter: NSDateFormatter = {
       let formatter = NSDateFormatter()
        formatter.dateStyle = .ShortStyle
        return formatter
    }()
    /// 耗时
    private lazy var calendar: NSCalendar = NSCalendar.currentCalendar()
    public class func normalAnimator() -> NormalAnimator {
        return NSBundle.mainBundle().loadNibNamed(String(NormalAnimator), owner: nil, options: nil).first as! NormalAnimator
    }
    public func setupDescriptionForState(closure: SetDescriptionClosure) {
        setupDesctiptionClosure = closure
    }
    public func setupLastFreshTime(closure: SetLastTimeClosure) {
        setupLastTimeClosure = closure
    }
    override public func awakeFromNib() {
        super.awakeFromNib()
        indicatorView.hidden = true
        indicatorView.hidesWhenStopped = true
    }
//    public override func layoutSubviews() {
//        super.layoutSubviews()
//        print("layout--------------------------------------------")
//    }
}
extension NormalAnimator: RefreshViewDelegate {
    public func refreshViewDidPrepare(refreshView: RefreshView, refreshType: RefreshViewType) {
        if refreshType == .header {
        } else {
            lastTimelabel.hidden = true
            rotateArrowToUpAnimated(false)
        }
        setupLastTime()
    }
    public func refreshDidBegin(refreshView: RefreshView, refreshViewType: RefreshViewType) {
        indicatorView.hidden = false
        indicatorView.startAnimating()
    }
    public func refreshDidEnd(refreshView: RefreshView, refreshViewType: RefreshViewType) {
        indicatorView.stopAnimating()
    }
    public func refreshDidChangeProgress(refreshView: RefreshView, progress: CGFloat, refreshViewType: RefreshViewType) {
        //        print(progress)
    }
    public func refreshDidChangeState(refreshView: RefreshView, fromState: RefreshViewState, toState: RefreshViewState, refreshViewType: RefreshViewType) {
        print(toState)
        setupDescriptionForState(toState, type: refreshViewType)
        switch toState {
        case .loading:
            imageView.hidden = true
        case .normal:
            setupLastTime()
            imageView.hidden = false
            ///恢复
            if refreshViewType == .header {
                rotateArrowToDownAnimated(false)
            } else {
                rotateArrowToUpAnimated(false)
            }
        case .pullToRefresh:
            if refreshViewType == .header {
                if fromState == .releaseToFresh {
                    rotateArrowToDownAnimated(true)
                }
            } else {
                if fromState == .releaseToFresh {
                    rotateArrowToUpAnimated(true)
                }
            }
            imageView.hidden = false
        case .releaseToFresh:
            imageView.hidden = false
            if refreshViewType == .header {
                rotateArrowToUpAnimated(true)
            } else {
                rotateArrowToDownAnimated(true)
            }
        }
    }
    private func setupDescriptionForState(state: RefreshViewState, type: RefreshViewType) {
        if descriptionLabel.hidden {
            descriptionLabel.text = ""
        } else {
            if let closure = setupDesctiptionClosure {
                descriptionLabel.text = closure(refreshState: state, refreshType: type)
            } else {
                switch state {
                case .normal:
                    descriptionLabel.text = "正常状态"
                case .loading:
                    descriptionLabel.text = "加载数据中..."
                case .pullToRefresh:
                    if type == .header {
                        descriptionLabel.text = "继续下拉刷新"
                    } else {
                        descriptionLabel.text = "继续上拉刷新"
                    }
                case .releaseToFresh:
                    descriptionLabel.text = "松开手刷新"
                }
            }
        }
    }
 }
使用方法
NormalAnimator

let normal = NormalAnimator.normalAnimator()
/// 指定存储刷新时间的key, 如果不指定或设置为nil, 那么将会和其他未指定的使用相同的key(记录的时间相同, MJRefresh是所有的控件使用相同的时间的)
normal.lastRefreshTimeKey = "DemoKey1"
/// 隐藏时间显示
//        normal.lastTimelabel.hidden = true
/// 自定义提示文字
//        normal.setupDescriptionForState { (refreshState,refreshType) -> String in
//            switch refreshState {
//            case .loading:
//                return "努力加载中"
//            case .normal:
//                return "休息中"
//            case .pullToRefresh:
//                if refreshType == .header {
//                    return "继续下下下下"
//
//                } else {
//                    return "继续上上上上"
//                }
//            case .releaseToFresh:
//                return "放开我"
//            };
//        }
/// 自定义时间显示
//        normal.setupLastFreshTime { (date) -> String in
//            return ...
//        }
tableView.zj_addRefreshHeader(normal, refreshHandler: {[weak self] in
/// 多线程中不要使用 [unowned self]
/// 注意这里的gcd是为了模拟网络加载的过程, 在实际的使用中, 不需要这段gcd代码, 直接在这里进行网络请求, 在请求完毕后, 调用分类方法, 结束刷新
dispatch_async(dispatch_get_global_queue(0, 0), {
for i in 0...50000 {
if i <= 10 {
self?.data.append(i)
}
/// 延时
print("加载数据中")
}
dispatch_async(dispatch_get_main_queue(), {
self?.tableView.reloadData()
/// 刷新完毕, 停止动画
self?.tableView.zj_stopHeaderAnimation()
})
})
})
GifAnimator的使用

/// 设置高度
let gifAnimatorHeader = GifAnimator.gifAnimatorWithHeight(100.0)
        gifAnimatorHeader.lastRefreshTimeKey = "exampleHeader4"
        /// 为不同的state设置不同的图片
        /// 闭包需要返回一个元组: 图片数组和gif动画每一帧的执行时间
        /// 一般需要设置loading状态的图片(必须), 作为加载的gif
        /// 和pullToRefresh状态的图片数组(可选择设置), 作为拖拽时的加载动画
        gifAnimatorHeader.setupImagesForRefreshstate { (refreshState) -> (images: [UIImage], duration: Double)? in
            if refreshState == .loading {
                var images = [UIImage]()
                for index in 1...47 {
                    let image = UIImage(named: "loading\\(index)")!
                    images.append(image)
                }
                return (images, 1.0)
            }
            else if  refreshState == .pullToRefresh {
                var images = [UIImage]()
                for index in 1...47 {
                    let image = UIImage(named: "loading\\(index)")!
                    images.append(image)
                }
                return (images, 0.25)
            }
            return nil
        }
        tableView.zj_addRefreshHeader(gif, refreshHandler: {[weak self] in
            /// 多线程中不要使用 [unowned self]
            /// 注意这里的gcd是为了模拟网络加载的过程, 在实际的使用中, 不需要这段gcd代码, 直接在这里进行网络请求, 在请求完毕后, 调用分类方法, 结束刷新
            dispatch_async(dispatch_get_global_queue(0, 0), {
                for i in 0...50000 {
                    if i <= 10 {
                        self?.data.append(i)
                    }
                    /// 延时
                    print("加载数据中")
                }
                dispatch_async(dispatch_get_main_queue(), {
                    self?.tableView.reloadData()
                    /// 刷新完毕, 停止动画
                    self?.tableView.zj_stopHeaderAnimation()
                })
            })
        })
或者你可以将这些自定义的设置移到另外新建的class中, 例如

class TestNormal {
class func normal() -> NormalAnimator {
let normal = NormalAnimator.normalAnimator()
/// 隐藏时间显示
//        normal.lastTimelabel.hidden = true
/// 指定存储刷新时间的key, 如果不指定或设置为nil, 那么将会和其他未指定的使用相同的key(记录的时间相同, MJRefresh是所有的控件使用相同的时间的)
normal.lastRefreshTimeKey = "DemoKey1"
normal.setupDescriptionForState({ (refreshState ,refreshType) -> String in
switch refreshState {
case .loading:
return "努力加载中"
case .normal:
return "休息中"
case .pullToRefresh:
if refreshType == .header {
return "继续下下下下"
} else {
return "继续上上上上"
}
case .releaseToFresh:
return "放开我"
}
})
return normal
}
}
/// 使用方法
let footer = TestNormal.normal()
tableView.zj_addRefreshFooter(footer) {[weak self] in
dispatch_async(dispatch_get_global_queue(0, 0), {
for i in 0...50000 {
if i <= 10 {
self?.data.append(i)
}
/// 延时
print("加载数据中")
}
dispatch_async(dispatch_get_main_queue(), {
self?.tableView.reloadData()
self?.tableView.zj_stopFooterAnimation()
})
})
}
总的来说, 简单写一个刷新控件还是很简单的, 但是在实现的过程中有很多的细节需要调整, 比如刷新的时候要处理sectionHeader的悬停问题... (这里直接借鉴了MJRefresh中的处理了), 。


## [Demo地址](https://github.com/mrscorpion/MSCustomRefresh)  
