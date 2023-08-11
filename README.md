import UIKit

class ViewController: UIViewController {

    var tableView: UITableView!

    override func viewDidLoad() {
        super.viewDidLoad()

        setupTableView()
    }

    func setupTableView() {
        tableView = UITableView(frame: view.bounds, style: .plain)
        tableView.register(TableViewCell.self, forCellReuseIdentifier: "TableViewCell")
        tableView.delegate = self
        tableView.dataSource = self
        view.addSubview(tableView)
    }
}

extension ViewController: UITableViewDataSource, UITableViewDelegate {
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return 1 // You can change this according to your data
    }

    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = tableView.dequeueReusableCell(withIdentifier: "TableViewCell", for: indexPath) as! TableViewCell
        return cell
    }

    func tableView(_ tableView: UITableView, heightForRowAt indexPath: IndexPath) -> CGFloat {
        let collectionViewHeight: CGFloat = UIScreen.main.bounds.width - 40 // Make the cell square
        let pageControlHeight: CGFloat = 16
        let cellHeight = collectionViewHeight + pageControlHeight + 40
        return cellHeight
    }
}

class TableViewCell: UITableViewCell {

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

        let collectionViewHeight: CGFloat = UIScreen.main.bounds.width - 40 // Make the cell square

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
        collectionView.isPagingEnabled = false // Disable paging
        collectionView.register(CollectionViewCell.self, forCellWithReuseIdentifier: "CollectionViewCell")
        contentView.addSubview(collectionView)
        
        NSLayoutConstraint.activate([
            collectionView.leadingAnchor.constraint(equalTo: contentView.leadingAnchor, constant: 20),
            collectionView.trailingAnchor.constraint(equalTo: contentView.trailingAnchor), // No trailing constraint
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
        pageControl.isUserInteractionEnabled = false // Desativa a interação com o UIPageControl
    }
}

extension TableViewCell: UICollectionViewDelegate, UICollectionViewDataSource, UICollectionViewDelegateFlowLayout {
    func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
        return cardColors.count
    }

    func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
        let cell = collectionView.dequeueReusableCell(withReuseIdentifier: "CollectionViewCell", for: indexPath) as! CollectionViewCell
        cell.backgroundColor = cardColors[indexPath.item]
        return cell
    }

    func scrollViewDidEndDecelerating(_ scrollView: UIScrollView) {
        let centerPoint = CGPoint(x: scrollView.contentOffset.x + scrollView.bounds.width / 2, y: scrollView.bounds.height / 2)
        if let indexPath = collectionView.indexPathForItem(at: centerPoint) {
            pageControl.currentPage = indexPath.item
            
            // Adjust for the last card
            if indexPath.item == cardColors.count - 1 {
                collectionView.scrollToItem(at: indexPath, at: .centeredHorizontally, animated: true)
            } else {
                collectionView.scrollToItem(at: indexPath, at: .centeredHorizontally, animated: true)
            }
        }
        else {
            // Calculate nearest index for center and adjust the content offset to center the card
            let centerOffsetX = scrollView.contentOffset.x + scrollView.bounds.width / 2
            let centerIndex = Int(centerOffsetX / contentView.bounds.width)
            
            let newOffsetX = CGFloat(centerIndex) * contentView.bounds.width
            scrollView.setContentOffset(CGPoint(x: newOffsetX, y: 0), animated: true)
            pageControl.currentPage = centerIndex
            
            // Scroll to the centered card with a snapping effect
            collectionView.scrollToItem(at: IndexPath(item: centerIndex, section: 0), at: .centeredHorizontally, animated: true)
        }
    }

}

class CollectionViewCell: UICollectionViewCell {
    override init(frame: CGRect) {
        super.init(frame: frame)
        commonInit()
    }

    required init?(coder aDecoder: NSCoder) {
        super.init(coder: aDecoder)
        commonInit()
    }

    private func commonInit() {
        layer.cornerRadius = 10 // Define o valor do raio de arredondamento
        layer.masksToBounds = true // Garante que o conteúdo dentro da célula não ultrapasse as bordas arredondadas
    }
}
