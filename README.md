    func tableView(_ tableView: UITableView, heightForRowAt indexPath: IndexPath) -> CGFloat {
        if indexPath.row == 0 {
            let collectionViewHeight: CGFloat = UIScreen.main.bounds.width - 40 // Make the cell square
            let pageControlHeight: CGFloat = 16
            let cellHeight = collectionViewHeight + pageControlHeight + 40
            return cellHeight
        } else {
            return 200
        }
    }
