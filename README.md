import UIKit

class MainOffersCarouselCollectionViewCell: UICollectionViewCell {

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
