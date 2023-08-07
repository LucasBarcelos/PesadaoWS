# PesadaoWS
class CarrosselViewController: UIViewController {
    private let collectionView: UICollectionView = {
        let layout = UICollectionViewFlowLayout()
        layout.scrollDirection = .horizontal
        layout.minimumLineSpacing = 8

        let collectionView = UICollectionView(frame: .zero, collectionViewLayout: layout)
        collectionView.translatesAutoresizingMaskIntoConstraints = false
        // Defina as propriedades do collectionView, como cores, tamanhos, etc.
        return collectionView
    }()

    // Outras propriedades e métodos da sua tela...
}

extension CarrosselViewController: UICollectionViewDataSource, UICollectionViewDelegate {
    func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
        // Retorne a quantidade de itens do carrossel aqui
        return 10
    }

    func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
        // Crie e retorne as células do carrossel aqui
        let cell = collectionView.dequeueReusableCell(withReuseIdentifier: "CarrosselCell", for: indexPath)
        // Configurar a célula com os dados apropriados
        return cell
    }
}

override func viewDidLoad() {
    super.viewDidLoad()

    collectionView.register(UICollectionViewCell.self, forCellWithReuseIdentifier: "CarrosselCell")
    collectionView.dataSource = self
    collectionView.delegate = self

    view.addSubview(collectionView)

    // Adicione as constraints para o collectionView, definindo as margens e espaçamentos desejados
    NSLayoutConstraint.activate([
        collectionView.topAnchor.constraint(equalTo: view.topAnchor, constant: 20),
        collectionView.leadingAnchor.constraint(equalTo: view.leadingAnchor, constant: 20),
        collectionView.trailingAnchor.constraint(equalTo: view.trailingAnchor, constant: -20),
        collectionView.heightAnchor.constraint(equalToConstant: 200) // Defina a altura desejada do carrossel
    ])
}

-------

class CarrosselCell: UICollectionViewCell {
    // Adicione as views necessárias, como a imagem de fundo e as duas labels
    let backgroundImageView: UIImageView = {
        let imageView = UIImageView()
        imageView.translatesAutoresizingMaskIntoConstraints = false
        // Configurar a imagem de fundo, preenchendo a célula inteira
        imageView.contentMode = .scaleAspectFill
        imageView.clipsToBounds = true
        return imageView
    }()

    let label1: UILabel = {
        let label = UILabel()
        label.translatesAutoresizingMaskIntoConstraints = false
        label.textColor = .white
        label.font = UIFont.boldSystemFont(ofSize: 24)
        label.textAlignment = .center
        // Defina outras propriedades da label 1, como o espaçamento, cor, etc.
        return label
    }()

    let label2: UILabel = {
        let label = UILabel()
        label.translatesAutoresizingMaskIntoConstraints = false
        label.textColor = .white
        label.font = UIFont.systemFont(ofSize: 18)
        label.textAlignment = .center
        // Defina outras propriedades da label 2, como o espaçamento, cor, etc.
        return label
    }()

    override init(frame: CGRect) {
        super.init(frame: frame)

        // Adicione as subviews à célula
        contentView.addSubview(backgroundImageView)
        contentView.addSubview(label1)
        contentView.addSubview(label2)

        // Adicione as constraints para as subviews, definindo os espaçamentos e tamanhos desejados
        NSLayoutConstraint.activate([
            backgroundImageView.topAnchor.constraint(equalTo: contentView.topAnchor),
            backgroundImageView.leadingAnchor.constraint(equalTo: contentView.leadingAnchor),
            backgroundImageView.trailingAnchor.constraint(equalTo: contentView.trailingAnchor),
            backgroundImageView.bottomAnchor.constraint(equalTo: contentView.bottomAnchor),
            
            label1.leadingAnchor.constraint(equalTo: contentView.leadingAnchor, constant: 16),
            label1.trailingAnchor.constraint(equalTo: contentView.trailingAnchor, constant: -16),
            label1.bottomAnchor.constraint(equalTo: contentView.bottomAnchor, constant: -24),
            
            label2.leadingAnchor.constraint(equalTo: contentView.leadingAnchor, constant: 16),
            label2.trailingAnchor.constraint(equalTo: contentView.trailingAnchor, constant: -16),
            label2.bottomAnchor.constraint(equalTo: label1.topAnchor, constant: -8),
        ])
    }

    required init?(coder aDecoder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }
}

override func viewDidLoad() {
    super.viewDidLoad()

    collectionView.register(CarrosselCell.self, forCellWithReuseIdentifier: "CarrosselCell")
    collectionView.dataSource = self
    collectionView.delegate = self

    // Restante do código...
}

extension CarrosselViewController: UICollectionViewDataSource, UICollectionViewDelegate {
    func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
        guard let cell = collectionView.dequeueReusableCell(withReuseIdentifier: "CarrosselCell", for: indexPath) as? CarrosselCell else {
            return UICollectionViewCell()
        }

        // Configurar a célula com os dados apropriados, como a imagem de fundo e conteúdo das labels
        cell.backgroundImageView.image = UIImage(named: "nome_da_imagem") // Substitua "nome_da_imagem" pelo nome da sua imagem
        cell.label1.text = "Texto da label 1"
        cell.label2.text = "Texto da label 2"

        return cell
    }

    // Restante do código...
}
