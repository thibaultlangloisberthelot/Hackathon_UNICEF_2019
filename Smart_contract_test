pragma solidity >=0.5.11 <0.9.99;
import "./MultiOwners.sol";


/**
 * 
 * 29/09/2019
 * 
 */
contract UNICEF is MultiOwners {
  using SafeMath for uint8;    

    uint256 public nbEnregistrements;
    uint256 public dateActivationContrat;
    
//  Indexation
    mapping ( uint256 => address ) private individusParIndex;
    mapping ( address => uint256 ) private indexParIndividus;
//  Phase 1
    mapping ( address => bytes32 ) private SMS;
//  Phase 2
    mapping ( address => bytes32 ) private appel1;
    mapping ( address => bytes32 ) private appel2;
    mapping ( address => bytes32 ) private appel3;
//  Journaux
    mapping ( address => mapping ( address => uint256 ) ) private accesDonnees;

    event IndividuCree ( uint256 datation, address adrIndividu );
    event SMSCree ( uint256 datation, address adrIndividu, bytes32 SMS );
    event AnnotCreees ( uint256 datation, bytes32 annotations, address adrIndividu );
    event Appel1Cree ( uint256 datation, address adrIndividu, bytes32 appel1 );
    event Appel2Cree ( uint256 datation, address adrIndividu, bytes32 appel2 );
    event Appel3Cree ( uint256 datation, address adrIndividu, bytes32 appel3 );
    
    
        /**
         * 
         * CONSTRUCTEUR
         */
        constructor() public {
            
            root = msg.sender;
            dateActivationContrat = now;
            nbEnregistrements = 0;
            
        }


    /**
     * 
     * Fonction INTERNE de journalisation.
     * 
     * @param _adrIndividu :
     */
    function _validation ( address _adrIndividu ) internal {
        accesDonnees[_adrIndividu][msg.sender] = now;
    }
    
//--------___________________________________________________________________________________
//  Seters
//________-----------------------------------------------------------------------------------

 
    /**
     *
     * Fonction de déclaration d'un individu.
     * 
     * @param _adrIndividu : Adress ETHEREUM de l'individu. 
     */
    function setIndividu ( address _adrIndividu ) public onlyRoots returns ( bool ) {
        indexParIndividus[msg.sender] = nbEnregistrements;
        individusParIndex[nbEnregistrements] = msg.sender;
        nbEnregistrements ++;
        
        _validation ( _adrIndividu );
        
        return true;
    }
    
    /**
     * 
     * Fonction d'enregistrement du SMS.
     * 
     * @param _adrIndividu : L'adresse Ethereum.
     * @param _SMS : Le hash du SMS.
     */
    function setSMS ( address _adrIndividu, bytes32 _SMS ) public onlyRoots {
        SMS[_adrIndividu] = _SMS;
        
        _validation ( _adrIndividu );
        
        emit SMSCree ( now, _adrIndividu, _SMS );
    }

    /**
     * 
     * Fonction d'enregistrement de l'appel n°1.
     * 
     * @param _adrIndividu : L'adresse Ethereum.
     * @param _appel1 : Le hash de l'enregistrement audio du premier appel.
     */
    function setPhase1 ( address _adrIndividu, bytes32 _appel1 ) public onlyRoots {
        appel1[_adrIndividu] = _appel1;
        
        _validation ( _adrIndividu );

        emit Appel1Cree ( now, _adrIndividu, _appel1 );
    }
    
    /**
     * 
     * Fonction d'enregistrement de l'appel n°2.
     * 
     * @param _adrIndividu : L'adresse Ethereum.
     * @param _appel2 : Le hash de l'enregistrement audio du deuxième appel.
     */
     function setPhase2 ( address _adrIndividu, bytes32 _appel2 ) public onlyRoots {
        appel2[_adrIndividu] = _appel2;
        
        _validation ( _adrIndividu );

        emit Appel2Cree ( now, _adrIndividu, _appel2 );
    }
    
    /**
     * 
     * Fonction d'enregistrement de l'appel n°3.
     * 
     * @param _adrIndividu : L'adresse Ethereum.
     * @param _appel3 : Le hash de l'enregistrement audio du troisième appel.
     */
    function setPhase3 ( address _adrIndividu, bytes32 _appel3 ) public onlyRoots {
        appel3[_adrIndividu] = _appel3;
        
        _validation ( _adrIndividu );

        emit Appel3Cree ( now, _adrIndividu, _appel3 );
    }
    
    
//--------___________________________________________________________________________________
//  Geters
//________-----------------------------------------------------------------------------------

    /**
     *
     * Fonction qui permet d'obtenir l'adresse Ethereum d'un individu par son n° d'index.
     * 
     * @param _indexIndividu : Le n° d'index de l'individu.
     */
    function getIndividuParIndex ( uint256 _indexIndividu ) public onlyRoots returns ( address _adrIndividu ) {
        _adrIndividu = individusParIndex[_indexIndividu];
        
        _validation ( _adrIndividu );
    }
    
    /**
     * 
     * Fonction qui permet d'obtenir l'index d'un individu par son adresse Ethereum.
     * 
     * @param _adrIndividu : L'adresse Ethereum de l'individu.
     */
    function getIndexParIndividu ( address _adrIndividu ) public onlyRoots returns ( uint256 _indexIndividu ) {
        _indexIndividu = indexParIndividus[_adrIndividu];
        
        _validation ( _adrIndividu );
    }
    
    /**
     * 
     * Fonction qui permet d'obtenir le hash du SMS correspondant à l'individu.
     * 
     * @param _adrIndividu : L'adresse Ethereum de l'individu.
     */
    function getSMS ( address _adrIndividu ) public onlyRoots returns ( bytes32 _SMS ) {
        _SMS = SMS[_adrIndividu];
        
        _validation ( _adrIndividu );
    }
    
    /**
     * 
     * Fonction qui permet d'obtenir le hash du premier appel passé.
     * 
     * @param _adrIndividu : L'adresse Ethereum de l'individu.
     */
    function getAppel1 ( address _adrIndividu ) public onlyRoots returns ( bytes32 _appel1 ) {
        _appel1 = appel1[_adrIndividu];
        
        _validation ( _adrIndividu );
    }
    
    /**
     * 
     * Fonction qui permet d'obtenir le hash du deuxième appel passé.
     * 
     * @param _adrIndividu : L'adresse Ethereum de l'individu.
     */
    function getAppel2 ( address _adrIndividu ) public onlyRoots returns ( bytes32 _appel2 ) {
        _appel2 = appel2[_adrIndividu];
        
        _validation ( _adrIndividu );
    }
    
    /**
     * 
     * Fonction qui permet d'obtenir le hash du troisième appel passé.
     * 
     * @param _adrIndividu : L'adresse Ethereum de l'individu.
     */
    function getAppel3 ( address _adrIndividu ) public onlyRoots returns ( bytes32 _appel3 ) {
        _appel3 = appel3[_adrIndividu];
        
        _validation ( _adrIndividu );
    }
    
//_-_-_-_-__-_-_-_-_----_-___--__-_-_-_-____----__--_--_-_-_-_-_-_-_-_-_-__-_--_-_-__-__-_-_-_-    
    
}
