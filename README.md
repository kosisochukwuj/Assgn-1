import java.util.Arrays;
import java.util.HashSet;
import java.util.Set;

public class TxHandler {

   private UTXOPool utxoPool;
   
public TxHandler(UTXOPool utxoPool) {
        this.utxoPool = new UTXOPool(utxoPool);
    }

public boolean isValidTx(Transaction tx){
        UTXOPool seenUTXOs = new UTXOPool();
    double consumedCoinsSum = 0;
    double valueProducedSum = 0;
    for (int i = 0; i < tx.numInputs(); i++) {
         Transaction.Input in = tx.getInput(i);
         UTXO utxo = new UTXO(in.prevTxHash, in.outputIndex);
         Transaction.Output output = utxoPool.getTxOutput(utxo);
         if (!utxoPool.contains(utxo))
         if (!Crypto.verifySignature(output.address, tx.getRawDataToSign(i), in.signature))
             return false;
         if (uniqueUTXOs.contains(utxo))
             return false;
            uniqueUTXOs.addUTXO(utxo, output);
            previousTxOutSum += output.value;
        }

   for (Transaction.Output out : tx.getOutputs()) {
            if (out.value < 0)
                return false;
            currentTxoutSum += out.value;
        }
        return previousTxOutSum >= currentTxOutSum;
    }

   public Transaction[] handleTxs(Transaction[] possibleTxs) {
        Set<Transaction> validTxs = new HashSet<>();
    
   for (Transaction tx :possibleTxs) {
        if (isValidTx(tx)) {
            validTxs.add(tx);
            for (Transaction.input in : tx.getInputs ()) {
                UTXO utxo = new UTXO(in.prevTXHash, in.outputIndex);
                utxoPool.removeUTXO(utxo);
                
   }
    for (int i = 0; i < tx.numOutputs(); i++) {
        Transaction.Output out = tx.getOutput(i);
        UTXO utxo = new UTXO(tx.getHash(), i);
        utxopool.addUTXO(utxo, out);
        }
   }
}      
    Transaction[] validTxArray = new Transaction[validTxs.size()];
     return validTxs.toArray(validTxArray);
   }
   
}
        
