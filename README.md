# favoritesLocalStore
import 'dart:io';
import 'package:flutter/material.dart';
import 'package:path/path.dart';
import 'package:path_provider/path_provider.dart';
import 'package:sqflite/sqflite.dart';

class HostelDatabase with ChangeNotifier {
  /// Singleton
  HostelDatabase._();

  static final HostelDatabase instance = HostelDatabase._();

  factory HostelDatabase() {
    return instance;
  }

  /// Table and column names
  static const String TABLE_HOSTEL = 'hostel';
  static const String COLUMN_HOSTEL_S_NO = 's_no';
  static const String COLUMN_HOSTEL_HOSTELNAME = 'hostelname';
  static const String COLUMN_HOSTEL_ADDRESS = 'address';
  static const String COLUMN_HOSTEL_IMAGE = 'image';
  static const String COLUMN_HOSTEL_RATING = 'rating';
  static const String COLUMN_HOSTEL_PRICE = 'price';
  static const String COLUMN_HOSTEL_MOBILE = 'mobile';
  static const String COLUMN_HOSTEL_DESCRIPTION = 'description';
  static const String COLUMN_HOSTEL_LATITUDE = 'latitude';
  static const String COLUMN_HOSTEL_LONGITUDE = 'longitude';
  static const String COLUMN_HOSTEL_SECURITY = 'security';
  static const String COLUMN_HOSTEL_HOSTELFOR = 'hostelfor';
  static const String COLUMN_HOSTEL_SINGLEROOMPRICE = 'singleroomprice';
  static const String COLUMN_HOSTEL_ISSINGLEROOMPRICE = 'issingleroomprice';
  static const String COLUMN_HOSTEL_DOUBLEROOMPRICE = 'doublebedprice';
  static const String COLUMN_HOSTEL_ISDOUBLEROOMPRICE = 'isdoublebedprice';
  static const String COLUMN_HOSTEL_TRIPLEROOMPRICE = 'triplebedprice';
  static const String COLUMN_HOSTEL_ISTRIPLEROOMPRICE = 'istriplebedprice';

  Database? myDB;

  /// Open or create the database
  Future<Database?> getDB() async {
    if (myDB != null) {
      return myDB;
    } else {
      myDB = await _openDB();
      return myDB;
    }
  }

  Future<Database?> _openDB() async {
    Directory directoryPath = await getApplicationDocumentsDirectory();
    String dbPath = join(directoryPath.path, "hostel.db");
    return await openDatabase(dbPath, version: 1, onCreate: (db, version) {
      return db.execute("CREATE TABLE $TABLE_HOSTEL("
          "$COLUMN_HOSTEL_S_NO INTEGER PRIMARY KEY AUTOINCREMENT, "
          "$COLUMN_HOSTEL_HOSTELNAME TEXT, "
          "$COLUMN_HOSTEL_ADDRESS TEXT, "
          "$COLUMN_HOSTEL_IMAGE TEXT, "
          "$COLUMN_HOSTEL_RATING TEXT, "
          "$COLUMN_HOSTEL_PRICE TEXT, "
          "$COLUMN_HOSTEL_MOBILE TEXT, "
          "$COLUMN_HOSTEL_DESCRIPTION TEXT, "
          "$COLUMN_HOSTEL_LATITUDE REAL, "
          "$COLUMN_HOSTEL_LONGITUDE REAL,$COLUMN_HOSTEL_HOSTELFOR TEXT, $COLUMN_HOSTEL_SECURITY TEXT, $COLUMN_HOSTEL_SINGLEROOMPRICE TEXT, $COLUMN_HOSTEL_DOUBLEROOMPRICE TEXT, $COLUMN_HOSTEL_TRIPLEROOMPRICE TEXT, $COLUMN_HOSTEL_ISSINGLEROOMPRICE TEXT,$COLUMN_HOSTEL_ISDOUBLEROOMPRICE TEXT, $COLUMN_HOSTEL_ISTRIPLEROOMPRICE TEXT)");
    });
  }

  /// Insert data
  Future<bool?> addData({
    required String hostelname,
    required String hosteladdress,
    required String hostelImage,
    required String hostelrating,
    required String hostelprice,
    required String hostelmobile,
    required String hosteldesc,
    required double hostellatitude,
    required double hostellongitude,
    required String hostelsecurity,
    required String hostelhostelfor,
    required String hostelsingleroomprice,
    required String hostelISsingleroomprice,
    required String hosteldoublebedprice,
    required String hostelISdoublebedprice,
    required String hosteltriplebedprice,
    required String hostelIStriplebedprice,
  }) async {
    var db = await getDB();
    int rowsAffected = await db!.insert(TABLE_HOSTEL, {
      COLUMN_HOSTEL_HOSTELNAME: hostelname,
      COLUMN_HOSTEL_ADDRESS: hosteladdress,
      COLUMN_HOSTEL_IMAGE: hostelImage,
      COLUMN_HOSTEL_RATING: hostelrating,
      COLUMN_HOSTEL_PRICE: hostelprice,
      COLUMN_HOSTEL_MOBILE: hostelmobile,
      COLUMN_HOSTEL_DESCRIPTION: hosteldesc,
      COLUMN_HOSTEL_LATITUDE: hostellatitude,
      COLUMN_HOSTEL_LONGITUDE: hostellongitude,
      COLUMN_HOSTEL_HOSTELFOR: hostelhostelfor,
      COLUMN_HOSTEL_SECURITY: hostelsecurity,
      COLUMN_HOSTEL_SINGLEROOMPRICE: hostelsingleroomprice,
      COLUMN_HOSTEL_DOUBLEROOMPRICE: hosteldoublebedprice,
      COLUMN_HOSTEL_TRIPLEROOMPRICE: hosteltriplebedprice,
      COLUMN_HOSTEL_ISSINGLEROOMPRICE: hostelISsingleroomprice,
      COLUMN_HOSTEL_ISDOUBLEROOMPRICE: hostelISdoublebedprice,
      COLUMN_HOSTEL_ISTRIPLEROOMPRICE: hostelIStriplebedprice,
    });
    return rowsAffected > 0; // true if data inserted
  }

  /// Read all data
  Future<List<Map<String, dynamic>>> getAllData() async {
    var db = await getDB();
    return await db!.query(TABLE_HOSTEL);
  }

  /// Remove data from favorites
  Future<bool?> removeData(String hostelname) async {
    var db = await getDB();
    int rowsAffected = await db!.delete(
      TABLE_HOSTEL,
      where: "$COLUMN_HOSTEL_HOSTELNAME = ?",
      whereArgs: [hostelname],
    );
    return rowsAffected > 0; // true if data removed
  }

  /// Check if a hostel is already in favorites
  Future<bool> isFavorite(String hostelname) async {
    var db = await getDB();
    final List<Map<String, dynamic>> result = await db!.query(
      TABLE_HOSTEL,
      where: "$COLUMN_HOSTEL_HOSTELNAME = ?",
      whereArgs: [hostelname],
    );
    return result.isNotEmpty; // true if the hostel exists
  }
}
