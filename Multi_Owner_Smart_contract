/// MultiOwners
//                                                          21/08/2019
/// Contrat qui permet la co-propriété d'un autre contrat si importé.

/// -Gère le propriétaire(root avec passation), les co-propriétaires (coRoots) aisni que les admins.
/// -Inclus l'éléments de sécurité 'Secu' et 2 fonctions de conversion de type, cf. fin du code.
//


pragma solidity >=0.5.11 <0.9.9;
import "./SafeMath.sol";


contract MultiOwners {
  using SafeMath for uint256;

/// Variables
    address payable public root;
    address payable public newRoot;

/// Mappages    
    mapping ( address => bool ) public coRoots;
    mapping ( address => bool ) public admins;

/// Evènements    
    event changedRoot ( address payable indexed _from, address payable indexed _to );

/// Modificateurs
    modifier secu {
        require ( msg.sender != address ( 0x0 ) );
        _;
    }
    
    modifier onlyRoots {
        require ( msg.sender != address ( 0x0 ) );
        require ( msg.sender == root || coRoots[msg.sender] );
        _;
    }
    
    modifier onlyAdmins {
        require ( msg.sender != address ( 0x0 ) );
        require ( msg.sender == root || coRoots[msg.sender] || admins[msg.sender] );
        _;        
    }
    
    modifier minimalCost ( uint256 _amount ) {
        require ( msg.value >= _amount );
        _;
    }


    /** 
     * 
     * Ajoute/enlève une adresse co-propriétaire au contrat.
     * 
     *@param _adrCoRoot: Adresse du co-propriétaire à traiter.
     *@param _yesNo: Active /désactive le status du co-propriétaire.  
     */
    function coRootsGestion ( address payable _adrCoRoot, bool _yesNo ) public onlyRoots {
        coRoots[_adrCoRoot] = _yesNo;
    }
    
    /**
     * 
     * Ajoute/enlève une adresse admin au contrat.
     * 
     *@param _adrAdmin: Adresse du co-propriétaire à traiter.
     *@param _yesNo: Active/désactive le status admin.  
     */
    function adminsGestion ( address payable _adrAdmin, bool _yesNo ) public onlyRoots {
        admins[_adrAdmin] = _yesNo;
    }

    /** 
     * 
     * Passation de pouvoir à un nouveau propriétaire.
     *Exécutée par le propriétaire actuel.
     * 
     *@param _newRoot: L'adresse Eth du nouveau propriétaire
     */
    function changeRoot ( address payable _newRoot ) public onlyRoots {
        newRoot = _newRoot;
    }
    
    /** 
     * 
     * Confirmation de la passation de pouvoir.
     *Exécutée par le nouveau propriétaire.
     */
    function confirmNewRoot() public {
        require ( msg.sender == newRoot );
        
        emit changedRoot ( root, newRoot );
        
        root = newRoot;
        delete newRoot;
    }

    /**
     * 
     * Fonction qui convertie un bytes32 en string.
     * 
     *@param _x: le bytes32 à convertir.
     */
    function bytes32ToString ( bytes32 _x ) public pure returns ( string memory ) {
        bytes memory _bytesString = new bytes ( 32 );
        uint _charCount = 0;
        
        for ( uint _i = 0; _i < 32; _i ++ ) {
            byte _char = byte ( bytes32 ( uint ( _x ) * 2 ** ( 8 * _i ) ) );
            if ( _char != 0 ) {
                _bytesString[_charCount] = _char;
                _charCount++;
            }
        }
        
        bytes memory _bytesStringTrimmed = new bytes ( _charCount );
        
        for ( uint _j = 0; _j < _charCount; _j++ ) {
            _bytesStringTrimmed[_j] = _bytesString[_j];
        }
        
        return string ( _bytesStringTrimmed );
    }

    /**
     * 
     * Fonction qui convertie un string en bytes32.
     * 
     *@param _source: String à convertir.
     */
    function stringToBytes32 ( string memory _source ) public pure returns ( bytes32 _result ) {
        bytes memory _tempEmptyStringTest = bytes(_source);
        
        if ( _tempEmptyStringTest.length == 0 ) {
            return 0x0;
        }
        
        assembly { _result := mload ( add ( _source, 32 ) ) }
    }


}
