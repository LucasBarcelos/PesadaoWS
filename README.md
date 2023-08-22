class OffersSantanderCollectionViewCell: UICollectionViewCell, WayViewCode {
    
    // MARK: - Properties
    static let identifier = "OffersSantanderCollectionViewCell"
    
    lazy var mainView: UIView = {
        let view = UIView()
        view.translatesAutoresizingMaskIntoConstraints = false
        view.layer.cornerRadius = 8
        view.layer.masksToBounds = true
        view.layer.shadowColor = UIColor.wayBlack.cgColor
        view.layer.shadowOpacity = 0.3
        view.layer.shadowOffset = CGSizeMake(0, 4)
        view.layer.shadowRadius = 4
        view.layer.masksToBounds = false
        return view
    }()
    
    private lazy var backgroundImage: UIImageView = {
        let image = UIImageView()
        image.translatesAutoresizingMaskIntoConstraints = false
        image.contentMode = .scaleAspectFit
        return image
    }()
    
    var trailingSpacingConstraint: NSLayoutConstraint?
    
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
            mainView.widthAnchor.constraint(equalToConstant: 162),
            mainView.heightAnchor.constraint(equalToConstant: 182),
            mainView.centerYAnchor.constraint(equalTo: centerYAnchor),
            mainView.centerXAnchor.constraint(equalTo: centerXAnchor),
            
            // backgroundImage
            backgroundImage.topAnchor.constraint(equalTo: mainView.topAnchor),
            backgroundImage.leadingAnchor.constraint(equalTo: mainView.leadingAnchor),
            backgroundImage.trailingAnchor.constraint(equalTo: mainView.trailingAnchor),
            backgroundImage.bottomAnchor.constraint(equalTo: mainView.bottomAnchor)
        ])
        
        trailingSpacingConstraint = mainView.trailingAnchor.constraint(equalTo: contentView.trailingAnchor)
    }
}




class OffersSantanderTableViewCell: UITableViewCell {
    
    // MARK: - Properties
    let layout = UICollectionViewFlowLayout()
    
    private lazy var titleLabel: UILabel = {
        let label = UILabel()
        label.translatesAutoresizingMaskIntoConstraints = false
        label.font = UIFont.boldSystemFont(ofSize: 16)
        label.text = "Ofertas Santander"
        label.textColor = .wayBlack
        label.numberOfLines = 0
        return label
    }()
    
    private lazy var subtitleLabel: UILabel = {
        let label = UILabel()
        label.translatesAutoresizingMaskIntoConstraints = false
        label.font = UIFont.systemFont(ofSize: 14)
        label.text = "Quem é Way, pode mais. Confira condições especiais."
        label.textColor = .wayBlack02
        label.numberOfLines = 0
        return label
    }()
    
    private lazy var collectionView: UICollectionView = {
        let collectionView = UICollectionView(frame: .zero, collectionViewLayout: layout)
        
        layout.scrollDirection = .horizontal
        layout.minimumLineSpacing = 8
        layout.itemSize = CGSize(width: 162, height: 182)
        
        collectionView.translatesAutoresizingMaskIntoConstraints = false
        collectionView.showsHorizontalScrollIndicator = false
        collectionView.delegate = self
        collectionView.dataSource = self
        collectionView.register(OffersSantanderCollectionViewCell.self, forCellWithReuseIdentifier: OffersSantanderCollectionViewCell.identifier)
        return collectionView
    }()
    
    var cardColors: [UIColor] = [.red, .green, .blue, .orange, .purple]
    
    // MARK: - inits
    override init(style: UITableViewCell.CellStyle, reuseIdentifier: String?) {
        super.init(style: style, reuseIdentifier: reuseIdentifier)
        setupSubviews()
        setupConstraints()
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
            collectionView.topAnchor.constraint(equalTo: subtitleLabel.bottomAnchor, constant: 8),
            collectionView.leadingAnchor.constraint(equalTo: contentView.leadingAnchor, constant: 20),
            collectionView.trailingAnchor.constraint(equalTo: contentView.trailingAnchor, constant: 0),
            collectionView.bottomAnchor.constraint(equalTo: contentView.bottomAnchor, constant: -12)
        ])
        
        let collectionViewHeightConstraint = collectionView.heightAnchor.constraint(equalToConstant: 200)
        collectionViewHeightConstraint.priority = .defaultLow
        collectionViewHeightConstraint.isActive = true
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
        
        // valida se é o último card para ativar o espaçamento
        if indexPath.item == cardColors.count - 1 {
            
            cell.trailingSpacingConstraint?.isActive = false
            cell.trailingSpacingConstraint = cell.mainView.trailingAnchor.constraint(equalTo: cell.contentView.trailingAnchor, constant: -20)
            cell.trailingSpacingConstraint?.isActive = true
            
        } else {
            cell.trailingSpacingConstraint?.isActive = true
        }
        return cell
    }
}
