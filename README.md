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
