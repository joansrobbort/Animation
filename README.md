{
        
        if sender.isOn == true {
            let ActivestardayUser = UserManager.getActiveUser()
            let copyUser = UserManager.copySelf(ActivestardayUser!)
            //   print(selctedmonth)
            copyUser?.settings?.balance_carry_over = true
            
            let result = UserManager.updateUser(copyUser!)
            
            if result.success == false {
                self.showAlert("Alert!", message: result.message)
            }
            
            UIView.animate(withDuration: 0.25, animations: {
                
                self.PositionViewHeight.constant = 35
               
                self.view.layoutIfNeeded()
                
            })
        }
        else
        {
            let ActivestardayUser = UserManager.getActiveUser()
            let copyUser = UserManager.copySelf(ActivestardayUser!)
            
            copyUser?.settings?.balance_carry_over = false
            
            let result = UserManager.updateUser(copyUser!)
            
            if result.success == false {
                self.showAlert("Alert!", message: result.message)
            }
            
            UIView.animate(withDuration: 0.25, animations: {
                self.PositionViewHeight.constant = 0
                self.view.layoutIfNeeded()
            })
        }
    }
    ==============================================================================================================
    let boolVal = UserDefaults.standard.value(forKey: "isFromLogin") as? Bool
        
        if boolVal != nil
        {
            if boolVal! {
                UserDefaults.standard.set(false, forKey: "isFromLogin")
                UserDefaults.standard.synchronize()
                self.view.removeFromSuperview()
               
            }else{
                let SpleCeVC = storyboard?.instantiateViewController(withIdentifier: "SplashViewController") as! SplashViewController
                self.view.addSubview(SpleCeVC.view)
                self.view.bringSubview(toFront: SpleCeVC.view)
                self.addChildViewController(SpleCeVC)
               
            }
        }else{
            let SpleCeVC = storyboard?.instantiateViewController(withIdentifier: "SplashViewController") as! SplashViewController
            self.view.addSubview(SpleCeVC.view)
            self.view.bringSubview(toFront: SpleCeVC.view)
            self.addChildViewController(SpleCeVC)
           
            
        }
        ==================================================================================================================
        
        
        ViewController
        
        
        
        import UIKit

class ViewController: UIViewController,ViewPagerDataSource {
   
    

    @IBOutlet weak var img: UIImageView!
    // @IBOutlet weak var imgview: UIImageView!
    //@IBOutlet weak var imgview: UIImageView!
    @IBOutlet weak var viewscrolling: ViewPager!
    var arrOfferImage = ["Image-10",
                         "Image-11",
                         "Image-12",
                         "Image-13",
                         "Image-14"]
    override func viewDidLoad() {
        super.viewDidLoad()
        self.setView()
    }
    
    override func viewWillAppear(_ animated: Bool) {
        super.viewWillAppear(animated)
        //var letterArray = [String]()
        for string in arrOfferImage {
            arrOfferImage.append(String(string.characters.first!))
            print(string)
            
            
        }
        
        
    }
    //MARK: - Other methods
        func setView() {
            
            self.subviewOfferImageSetUp()
        }
    
    func numberOfItems(viewPager: ViewPager) -> Int {
        return arrOfferImage.count
    }
    
    func viewAtIndex(viewPager: ViewPager, index: Int, view: UIView?) -> UIView {
        let objHeaderViewPagerVC = hederVC(nibName: "hederVC", bundle: nil)
        objHeaderViewPagerVC.imageName = arrOfferImage[index]
        return objHeaderViewPagerVC.view
    }
        
        func subviewOfferImageSetUp() {
            self.viewscrolling.dataSource = self
            DispatchQueue.main.async {
                self.ImagePageViewSetup()
            }
        }
        
        func ImagePageViewSetup() {
            self.viewscrolling.scrollViewDidEndDecelerating(self.viewscrolling.scrollView)
        }
    
  


}
===================================================================================================================================Xib Headview

class hederVC: UIViewController {

    @IBOutlet weak var imgView: UIImageView!
    var itemIndex: Int = 0
    
    var imageName: String = "" {
        didSet {
            if let imageView = imgView {
                imageView.image = UIImage(named: imageName)
            }
        }
    }
    
    override func viewDidLoad() {
        super.viewDidLoad()
        self.setView()
    }
    
    
    
    func setView() {
        self.imgView.layer.cornerRadius = 5.5
        self.imgView.image = UIImage(named: imageName)
    }


}
======================================================================================================================================


FIrst ViewController 

func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {
        //StatesBar
        UIApplication.shared.statusBarView?.backgroundColor = COLOR_COMMON_DARK_ORNAGE
        
        let timeStampSiceNow = Int((Date().timeIntervalSince1970 * 1000.0).rounded())
        print(timeStampSiceNow)
        
        
        window = UIWindow(frame: UIScreen.main.bounds)
        appDelegate.window?.backgroundColor = COLOR_COMMON_DARK_ORNAGE
        
        
        let storybord = UIStoryboard.init(name: "Main2", bundle: nil)
        var mainViewController = UIViewController()
       
        if let Is_LogIn = Pref.getObjectForKey(kISLogInUser) as? Bool
        , Is_LogIn == true {
            mainViewController = (storybord.instantiateViewController(withIdentifier: "HomeVC") as? HomeVC)!
        } else {
            mainViewController = (storybord.instantiateViewController(withIdentifier: "LoginViewController") as? LoginViewController)!
        }
            
        
        let leftViewController = UIStoryboard.init(name: "ProductDetaileMain", bundle: nil).instantiateViewController(withIdentifier: "LeftDrawerVC") as? LeftDrawerVC
        
        let nvc: UINavigationController = UINavigationController(rootViewController: mainViewController)
        nvc.setNavigationBarHidden(true, animated: true)
        
        leftViewController?.mainVC = nvc
        
        let slideMenuController = ExSlideMenuController(mainViewController:nvc, leftMenuViewController: leftViewController!)
        slideMenuController.automaticallyAdjustsScrollViewInsets = true
        
        self.window?.backgroundColor = UIColor.white
        self.window?.rootViewController = slideMenuController
        
        application.isStatusBarHidden = false
        self.window?.makeKeyAndVisible()
        
        INITIAL_SETUP()
        return true
    }
    
   
   

}
===================================================================================================================



    
    
    
