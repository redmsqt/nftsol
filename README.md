// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

// Importujemy gotowy i bezpieczny kontrakt ERC721 od OpenZeppelin
import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

// Nasz kontrakt dziedziczy wszystkie funkcje z kontraktu ERC721
contract MyFirstNFT is ERC721, Ownable {
    
    // Licznik, aby każdy nowy NFT miał unikalne ID
    uint256 private _nextTokenId;

    // Konstruktor uruchamia się tylko raz, podczas wdrożenia kontraktu
    constructor() ERC721("My First NFT Collection", "MFNFT") {}

    /**
     * @dev Publiczna funkcja, która pozwala każdemu stworzyć (mint) nowy NFT.
     * Wymaga podania adresu portfela, na który NFT ma trafić.
     */
    function safeMint(address to) public {
        uint256 tokenId = _nextTokenId++;
        _safeMint(to, tokenId);
    }
    
    /**
     * @dev Funkcja tylko dla właściciela kontraktu do wycofania środków.
     */
    function withdraw() public payable onlyOwner {
        (bool success, ) = payable(owner()).call{value: address(this).balance}("");
        require(success);
    }
}
