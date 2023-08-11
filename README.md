class MainOffersCarouselCollectionViewCell: UICollectionViewCell, WayViewCode {

    // MARK: - Properties
    static let identifier = "MainOffersCarouselCollectionViewCell"
    
    private lazy var mainView: UIView = {
        let view = UIView()
        view.translatesAutoresizingMaskIntoConstraints = false
        return view
    }()
    
    private lazy var backgroundImage: UIImageView = {
        let image = UIImageView()
        image.translatesAutoresizingMaskIntoConstraints = false
        image.contentMode = .scaleAspectFit
        return image
    }()
    
    private lazy var titleLabel: UILabel = {
        let label = UILabel()
        label.translatesAutoresizingMaskIntoConstraints = false
        label.font = UIFont.boldSystemFont(ofSize: 18)
        label.textColor = .white
        return label
    }()
    
    private lazy var subtitleLabel: UILabel = {
        let label = UILabel()
        label.translatesAutoresizingMaskIntoConstraints = false
        label.font = UIFont.systemFont(ofSize: 14)
        label.textColor = .white
        return label
    }()
    
    // MARK: - Inits
    override init(frame: CGRect) {
        super.init(frame: frame)
        setup()
    }
    
    required init?(coder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }
    
    // MARK: - Methods
    override func prepareForReuse() {
        super.prepareForReuse()
        backgroundImage.image = nil
        titleLabel.text = nil
        subtitleLabel.text = nil
    }
    
    func buildViewHierarchy() {
        addSubview(mainView)
        mainView.addSubview(backgroundImage)
        mainView.addSubview(titleLabel)
        mainView.addSubview(subtitleLabel)
    }
    
    func setupAdditionalConfiguration() {
        mainView.layer.cornerRadius = 10
        mainView.layer.masksToBounds = true
    }
    
    func setupConstraints() {
        NSLayoutConstraint.activate([
            // mainView
            mainView.topAnchor.constraint(equalTo: topAnchor),
            mainView.leadingAnchor.constraint(equalTo: leadingAnchor),
            mainView.trailingAnchor.constraint(equalTo: trailingAnchor),
            mainView.bottomAnchor.constraint(equalTo: bottomAnchor),
            
            // backgroundImage
            backgroundImage.topAnchor.constraint(equalTo: mainView.topAnchor),
            backgroundImage.leadingAnchor.constraint(equalTo: mainView.leadingAnchor),
            backgroundImage.trailingAnchor.constraint(equalTo: mainView.trailingAnchor),
            backgroundImage.bottomAnchor.constraint(equalTo: mainView.bottomAnchor),
            
            // titleLabel
            titleLabel.leadingAnchor.constraint(equalTo: mainView.leadingAnchor, constant: 16),
            titleLabel.trailingAnchor.constraint(equalTo: mainView.trailingAnchor, constant: -16),
            titleLabel.bottomAnchor.constraint(equalTo: subtitleLabel.topAnchor, constant: -8),
            
            // subtitleLabel
            subtitleLabel.leadingAnchor.constraint(equalTo: mainView.leadingAnchor, constant: 16),
            subtitleLabel.trailingAnchor.constraint(equalTo: mainView.trailingAnchor, constant: -16),
            subtitleLabel.bottomAnchor.constraint(equalTo: mainView.bottomAnchor, constant: -24)
        ])
    }
}


class MainOffersCarouselTableViewCell: UITableViewCell {
    
    var collectionView: UICollectionView!
    var pageControl: UIPageControl!
    
    let cardColors: [UIColor] = [.red, .green, .blue, .orange, .purple]
    
    override init(style: UITableViewCell.CellStyle, reuseIdentifier: String?) {
        super.init(style: style, reuseIdentifier: reuseIdentifier)
        
        setupCollectionView()
        setupPageControl()
    }
    
    required init?(coder aDecoder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }
    
    override func layoutSubviews() {
        super.layoutSubviews()
        
        let collectionViewHeight: CGFloat = UIScreen.main.bounds.width - 40
        
        collectionView.frame = CGRect(x: 20, y: 20, width: contentView.bounds.width - 40, height: collectionViewHeight)
        pageControl.frame = CGRect(x: 0, y: collectionView.frame.maxY + 20, width: contentView.bounds.width, height: 16)
    }
    
    func setupCollectionView() {
        let layout = UICollectionViewFlowLayout()
        layout.scrollDirection = .horizontal
        layout.minimumLineSpacing = 8
        
        let cardSize = CGSize(width: contentView.bounds.width, height: contentView.bounds.width)
        layout.itemSize = cardSize
        
        collectionView = UICollectionView(frame: CGRect.zero, collectionViewLayout: layout)
        collectionView.translatesAutoresizingMaskIntoConstraints = false
        collectionView.showsHorizontalScrollIndicator = false
        collectionView.delegate = self
        collectionView.dataSource = self
        collectionView.isPagingEnabled = false
        collectionView.register(MainOffersCarouselCollectionViewCell.self, forCellWithReuseIdentifier: "MainOffersCarouselCollectionViewCell")
        contentView.addSubview(collectionView)
        
        NSLayoutConstraint.activate([
            collectionView.leadingAnchor.constraint(equalTo: contentView.leadingAnchor, constant: 20),
            collectionView.trailingAnchor.constraint(equalTo: contentView.trailingAnchor),
            collectionView.topAnchor.constraint(equalTo: contentView.topAnchor, constant: 20),
            collectionView.bottomAnchor.constraint(equalTo: contentView.bottomAnchor, constant: -20)
        ])
    }
    
    func setupPageControl() {
        let pageControlHeight: CGFloat = 16
        
        pageControl = UIPageControl()
        pageControl.translatesAutoresizingMaskIntoConstraints = false
        pageControl.pageIndicatorTintColor = .gray
        pageControl.currentPageIndicatorTintColor = .red
        contentView.addSubview(pageControl)
        
        NSLayoutConstraint.activate([
            pageControl.centerXAnchor.constraint(equalTo: contentView.centerXAnchor),
            pageControl.topAnchor.constraint(equalTo: collectionView.bottomAnchor, constant: 0),
            pageControl.heightAnchor.constraint(equalToConstant: pageControlHeight)
        ])
        
        pageControl.numberOfPages = cardColors.count
        pageControl.isUserInteractionEnabled = false
    }
}

extension MainOffersCarouselTableViewCell: UICollectionViewDelegate, UICollectionViewDataSource, UICollectionViewDelegateFlowLayout {
    
    func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
        return cardColors.count
    }
    
    func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
        let cell = collectionView.dequeueReusableCell(withReuseIdentifier: "MainOffersCarouselCollectionViewCell", for: indexPath) as! MainOffersCarouselCollectionViewCell
        cell.backgroundColor = cardColors[indexPath.item]
        return cell
    }
    
    func scrollViewDidEndDecelerating(_ scrollView: UIScrollView) {
        let centerPoint = CGPoint(x: scrollView.contentOffset.x + scrollView.bounds.width / 2, y: scrollView.bounds.height / 2)
        if let indexPath = collectionView.indexPathForItem(at: centerPoint) {
            pageControl.currentPage = indexPath.item
            
            if indexPath.item == cardColors.count - 1 {
                collectionView.scrollToItem(at: indexPath, at: .centeredHorizontally, animated: true)
            } else {
                collectionView.scrollToItem(at: indexPath, at: .centeredHorizontally, animated: true)
            }
        }
        else {
            let centerOffsetX = scrollView.contentOffset.x + scrollView.bounds.width / 2
            let centerIndex = Int(centerOffsetX / contentView.bounds.width)
            
            let newOffsetX = CGFloat(centerIndex) * contentView.bounds.width
            scrollView.setContentOffset(CGPoint(x: newOffsetX, y: 0), animated: true)
            pageControl.currentPage = centerIndex
            
            collectionView.scrollToItem(at: IndexPath(item: centerIndex, section: 0), at: .centeredHorizontally, animated: true)
        }
    }
}



class NewInteractOffersView: UIView, WayViewCode {

    // MARK: - Properties
    private lazy var mainView: UIView = {
        let view = UIView()
        view.backgroundColor = .santanderRed()
        view.translatesAutoresizingMaskIntoConstraints = false
        return view
    }()
    
    private lazy var contentView: UIView = {
        let view = UIView()
        view.backgroundColor = .wayWhite
        view.layerCornerRadius = 10
        view.layer.masksToBounds = true
        view.translatesAutoresizingMaskIntoConstraints = false
        return view
    }()
    
    lazy var tableView: UITableView = {
        let tableView = UITableView()
        tableView.translatesAutoresizingMaskIntoConstraints = false
        tableView.separatorStyle = .none
        tableView.backgroundColor = .clear
        return tableView
    }()
    
    // MARK: - Inits
    override init(frame: CGRect) {
        super.init(frame: frame)
        self.setupBackground()
        setup()
    }
    
    required init?(coder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }
    
    // MARK: - Methods
    func setupBackground() {
        self.backgroundColor = .wayWhite
    }
    
    func buildViewHierarchy() {
        self.addSubview(self.mainView)
        self.addSubview(self.contentView)
        self.addSubview(self.tableView)
    }
    
    func setupConstraints() {
        NSLayoutConstraint.activate([
            
            mainView.topAnchor.constraint(equalTo: self.safeAreaLayoutGuide.topAnchor),
            mainView.leadingAnchor.constraint(equalTo: self.safeAreaLayoutGuide.leadingAnchor),
            mainView.trailingAnchor.constraint(equalTo: self.safeAreaLayoutGuide.trailingAnchor),
            mainView.bottomAnchor.constraint(equalTo: self.safeAreaLayoutGuide.bottomAnchor),
            
            //contentView
            contentView.topAnchor.constraint(equalTo: self.safeAreaLayoutGuide.topAnchor),
            contentView.leadingAnchor.constraint(equalTo: self.leadingAnchor),
            contentView.trailingAnchor.constraint(equalTo: self.trailingAnchor),
            contentView.bottomAnchor.constraint(equalTo: self.bottomAnchor, constant: +10),
            
            // tableView
            tableView.topAnchor.constraint(equalTo: self.safeAreaLayoutGuide.topAnchor),
            tableView.leadingAnchor.constraint(equalTo: self.safeAreaLayoutGuide.leadingAnchor),
            tableView.trailingAnchor.constraint(equalTo: self.safeAreaLayoutGuide.trailingAnchor),
            tableView.bottomAnchor.constraint(equalTo: self.safeAreaLayoutGuide.bottomAnchor)
        ])
    }
}


class NewInteractOffersViewController: UIViewController  {

    // MARK: - Properties
    var offersScreen: NewInteractOffersView?

    override func loadView() {
        self.offersScreen = NewInteractOffersView()
        self.view = self.offersScreen
    }
    
    // MARK: - View Life Cycle
    override func viewDidLoad() {
        super.viewDidLoad()
        self.title = "Ofertas"
        
        // Define a cor da navigation bar
        navigationController?.navigationBar.barTintColor = .santanderRed()
        self.navigationController?.navigationBar.isTranslucent = false
        navigationController?.wayStandardNavigationController()
        
        setupTableView()
    }
    
    // MARK: - Methods
    func setupTableView() {
        self.offersScreen?.tableView = UITableView(frame: view.bounds, style: .plain)
        self.offersScreen?.tableView.register(MainOffersCarouselTableViewCell.self, forCellReuseIdentifier: "MainOffersCarouselTableViewCell")
        self.offersScreen?.tableView.delegate = self
        self.offersScreen?.tableView.dataSource = self
    }
}

extension NewInteractOffersViewController: UITableViewDataSource, UITableViewDelegate {
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return 1
    }
    
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = tableView.dequeueReusableCell(withIdentifier: "MainOffersCarouselTableViewCell", for: indexPath) as! MainOffersCarouselTableViewCell
        return cell
    }
    
    func tableView(_ tableView: UITableView, heightForRowAt indexPath: IndexPath) -> CGFloat {
        let collectionViewHeight: CGFloat = UIScreen.main.bounds.width - 40 // Make the cell square
        let pageControlHeight: CGFloat = 16
        let cellHeight = collectionViewHeight + pageControlHeight + 40
        return cellHeight
    }
}
