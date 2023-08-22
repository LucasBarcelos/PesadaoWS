UIViewController:
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


UIView:
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
            tableView.topAnchor.constraint(equalTo: self.safeAreaLayoutGuide.topAnchor, constant: 10),
            tableView.leadingAnchor.constraint(equalTo: self.safeAreaLayoutGuide.leadingAnchor),
            tableView.trailingAnchor.constraint(equalTo: self.safeAreaLayoutGuide.trailingAnchor),
            tableView.bottomAnchor.constraint(equalTo: self.safeAreaLayoutGuide.bottomAnchor)
        ])
    }
}

UITableViewCell:
class MainOffersCarouselTableViewCell: UITableViewCell {
    
    // MARK: - Properties
    var collectionView: UICollectionView!
    var pageControl: UIPageControl!
    let cardColors: [UIColor] = [.red, .green, .blue, .orange, .purple]
    
    // MARK: - Inits
    override init(style: UITableViewCell.CellStyle, reuseIdentifier: String?) {
        super.init(style: style, reuseIdentifier: reuseIdentifier)
        
        setupCollectionView()
        setupPageControl()
    }
    
    required init?(coder aDecoder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }
    
    // MARK: - Methods
    override func layoutSubviews() {
        super.layoutSubviews()
        
        collectionView.frame = CGRect(x: 0, y: 0, width: contentView.bounds.width - 40, height: contentView.bounds.width)
        pageControl.frame = CGRect(x: 0, y: collectionView.frame.maxY, width: contentView.bounds.width, height: 16)
    }
    
    func setupCollectionView() {
        let layout = UICollectionViewFlowLayout()
        layout.scrollDirection = .horizontal
        layout.minimumLineSpacing = 10
        
        let cardSize = CGSize(width: contentView.bounds.width + 40, height: contentView.bounds.width + 60)
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
            collectionView.topAnchor.constraint(equalTo: contentView.topAnchor),
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

// MARK: - Extensions
extension MainOffersCarouselTableViewCell: UICollectionViewDelegate, UICollectionViewDataSource, UICollectionViewDelegateFlowLayout {
    
    func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
        return cardColors.count
    }
    
    func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
        let cell = collectionView.dequeueReusableCell(withReuseIdentifier: "MainOffersCarouselCollectionViewCell", for: indexPath) as! MainOffersCarouselCollectionViewCell
        cell.mainView.backgroundColor = cardColors[indexPath.item]
        return cell
    }
    
    func scrollViewDidEndDecelerating(_ scrollView: UIScrollView) {
        let centerPoint = CGPoint(x: scrollView.contentOffset.x + scrollView.bounds.width / 2, y: scrollView.bounds.height / 2)
        if let indexPath = collectionView.indexPathForItem(at: centerPoint) {
            pageControl.currentPage = indexPath.item
            
            if indexPath.item == cardColors.count - 1 {
                // último card: ajusta o trailing para centralizar
                let lastItemWidth = collectionView.bounds.width - contentView.bounds.width + 20
                let offset = (collectionView.bounds.width - lastItemWidth) / 2
                collectionView.contentInset = UIEdgeInsets(top: 0, left: offset, bottom: 0, right: offset)
            } else {
                // restaura o trailling nos demais cards
                collectionView.contentInset = UIEdgeInsets.zero
            }
            
            // centraliza o card selecionado
            collectionView.scrollToItem(at: indexPath, at: .centeredHorizontally, animated: true)
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

UICollectionViewCell:
class MainOffersCarouselCollectionViewCell: UICollectionViewCell, WayViewCode {

    // MARK: - Properties
    static let identifier = "MainOffersCarouselCollectionViewCell"
    
    lazy var mainView: UIView = {
        let view = UIView()
        view.layer.cornerRadius = 10
        view.layer.masksToBounds = true
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


///////////////////////////







class OffersSantanderCollectionViewCell: UICollectionViewCell, WayViewCode {
    
    // MARK: - Properties
    static let identifier = "OffersSantanderCollectionViewCell"
    
    lazy var mainView: UIView = {
        let view = UIView()
        view.layer.cornerRadius = 8
        view.layer.masksToBounds = true
        view.translatesAutoresizingMaskIntoConstraints = false
        return view
    }()
    
    private lazy var backgroundImage: UIImageView = {
        let image = UIImageView()
        image.translatesAutoresizingMaskIntoConstraints = false
        image.contentMode = .scaleAspectFit
        return image
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
    }
    
    func buildViewHierarchy() {
        addSubview(mainView)
        mainView.addSubview(backgroundImage)
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
            backgroundImage.bottomAnchor.constraint(equalTo: mainView.bottomAnchor)
        ])
    }
}













class OffersSantanderTableViewCell: UITableViewCell {
    
    // MARK: - Properties
    let layout = UICollectionViewFlowLayout()
    
    private lazy var titleLabel: UILabel = {
        let label = UILabel()
        label.translatesAutoresizingMaskIntoConstraints = false
        label.font = UIFont.boldSystemFont(ofSize: 18)
        label.text = "Ofertas Santander"
        label.textColor = .white
        return label
    }()
    
    private lazy var subtitleLabel: UILabel = {
        let label = UILabel()
        label.translatesAutoresizingMaskIntoConstraints = false
        label.font = UIFont.systemFont(ofSize: 14)
        label.text = "Quem é Way, pode mais. Confira condições especiais."
        label.textColor = .white
        return label
    }()
    
    private lazy var collectionView: UICollectionView = {
        let collectionView = UICollectionView()
        
        layout.scrollDirection = .horizontal
        layout.minimumLineSpacing = 10
        layout.itemSize = CGSize(width: 162, height: 182)

        collectionView.collectionViewLayout = layout
        collectionView.translatesAutoresizingMaskIntoConstraints = false
        collectionView.showsHorizontalScrollIndicator = false
        collectionView.delegate = self
        collectionView.dataSource = self
        collectionView.register(OffersSantanderTableViewCell.self, forCellWithReuseIdentifier: "OffersSantanderTableViewCell")
    }()
    
    var cardColors: [UIColor] = [.red, .green, .blue, .orange, .purple]
    
    // MARK: - inits
    override init(style: UITableViewCell.CellStyle, reuseIdentifier: String?) {
        super.init(style: style, reuseIdentifier: reuseIdentifier)
        setupSubviews()
        setupConstraints()
        setupCollectionView()
    }
    
    required init?(coder aDecoder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }
    
    // MARK: - Methods
    func setupSubviews() {
        contentView.addSubview(titleLabel)
        contentView.addSubview(subtitleLabel)
        contentView.addSubview(collectionView)
    }
    
    func setupConstraints() {
        NSLayoutConstraint.activate([
            // titleLabel
            titleLabel.topAnchor.constraint(equalTo: contentView.topAnchor, constant: 20),
            titleLabel.leadingAnchor.constraint(equalTo: contentView.leadingAnchor, constant: 20),
            titleLabel.trailingAnchor.constraint(equalTo: contentView.trailingAnchor, constant: -20),
            
            // subtitleLabel
            subtitleLabel.topAnchor.constraint(equalTo: titleLabel.bottomAnchor, constant: 8),
            subtitleLabel.leadingAnchor.constraint(equalTo: contentView.leadingAnchor, constant: 20),
            subtitleLabel.trailingAnchor.constraint(equalTo: contentView.trailingAnchor, constant: -20),
            
            // collectionView
            collectionView.leadingAnchor.constraint(equalTo: contentView.leadingAnchor, constant: 20),
            collectionView.trailingAnchor.constraint(equalTo: contentView.trailingAnchor),
            collectionView.topAnchor.constraint(equalTo: subtitleLabel.bottomAnchor, constant: 8),
            collectionView.bottomAnchor.constraint(equalTo: contentView.bottomAnchor, constant: -12)
        ])
    }
}

// MARK: - Extensions
extension OffersSantanderTableViewCell: UICollectionViewDelegate, UICollectionViewDataSource, UICollectionViewDelegateFlowLayout {
    
    func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
        return cardColors.count
    }
    
    func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
        let cell = collectionView.dequeueReusableCell(withReuseIdentifier: "OffersSantanderCollectionViewCell", for: indexPath) as! OffersSantanderCollectionViewCell
        cell.mainView.backgroundColor = cardColors[indexPath.item]
        return cell
    }
}
